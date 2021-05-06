---
layout:     post   				    # 使用的布局（不需要改）
title:      Unity的换装系统		   	# 标题 
subtitle:             # 副标题
date:       2021-05-05 				# 时间
author:     Nathan 				# 作者
header-img: img/post-bg-unity-dev.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Unity开发
---

# Unity换装系统

## 引言

之前的项目需要实现人物的换装系统，网上目前的教程大部分都是骨骼点替换+Mesh合并的方案，代码也相对老旧。这里记录一下自己写的仅基于骨骼点替换的换装系统。

这里先简单讲一下两个换装系统用到的技术（仅为个人理解）

1. 骨骼点(Skeleton): 骨骼点就是附在模型上的Transform point，通过改动骨骼点的position，rotation或者scale可以对其对应的蒙皮进行位移，旋转或者缩放
2. 蒙皮（Skinned Mesh): 和骨骼点绑定的mesh。

所以换装系统需要一个具有骨骼点绑定模型，并且可以更换的服装也和主体模型有一样的骨骼点。接着进行骨骼点替换，把服装的骨骼点替换程主体模型的骨骼点。这样Unity在播放主体模型的动画时（实际就是对主体模型的骨骼点进行transform），服装也会跟着一起动，这就完成了简单的换装。

## 实现
->[完整项目](https://github.com/moecia/UnityClothesSample)<-
### SkinnedMeshHelper.cs
这个class主要就是按照我在引言中提到的思路进行骨骼点查找，然后返回一个新的骨骼点array供替换。
```
using System.Linq;
using UnityEngine;

public static class SkinnedMeshHelper
{
    public static Transform[] GetNewBones(SkinnedMeshRenderer root, SkinnedMeshRenderer source)
    {
        return root.bones
            .Where(x => source.bones.Select(s => s.name).Contains(x.name)).ToArray();
    }
}
```
### Outfit.cs
这个class放在服装的prefab上，设定好OutfitType（Hair,Cloth,Pant,Shoes）后放在Resources/Outfit/{OutfitType}/下。文件名和**Id**保持一致。
```
using UnityEngine;

public class Outfit : MonoBehaviour
{
    [SerializeField] private OutfitType outfitType;
    private SkinnedMeshRenderer skinnedMeshRenderer;

    public OutfitType OutfitType { get => outfitType; set => outfitType = value; }
    public int Id { get => int.Parse(this.name); }
    public SkinnedMeshRenderer SkinnedMeshRenderer
    {
        get
        {
            if (skinnedMeshRenderer == null)
            {
                skinnedMeshRenderer = this.GetComponentInChildren<SkinnedMeshRenderer>();
            }
            return skinnedMeshRenderer;
        }
    }
}

public enum OutfitType
{
    Hair,
    Cloth,
    Pant,
    Shoes
}
```
### EquipmentManager.cs
这个类用于换装前端逻辑，使用前先在人物身上放4个Slot，分别对应头，身，腿和脚。
```
using UnityEngine;

public class EquipmentManager : MonoBehaviour
{
    [SerializeField] private Transform hairSlot;
    [SerializeField] private Transform clothSlot;
    [SerializeField] private Transform pantSlot;
    [SerializeField] private Transform shoesSlot;
    [SerializeField] private SkinnedMeshRenderer avatarSkinnedMesh;

    public Transform HairSlot { get => hairSlot; }
    public Transform ClothSlot { get => clothSlot; }
    public Transform PantSlot { get => pantSlot; }
    public Transform ShoesSlot { get => shoesSlot; }

    public int HairId { get; set; }
    public int ClothId { get; set; }
    public int PantId { get; set; }
    public int ShoesId { get; set; }

    public void LoadEquipment()
    {
        ChangeOutfit(OutfitType.Hair, 1);
        ChangeOutfit(OutfitType.Cloth, 1);
        ChangeOutfit(OutfitType.Pant, 1);
        ChangeOutfit(OutfitType.Shoes, 1);
    }

    public void ChangeOutfit(OutfitType outfitType, int outfitId)
    {
        GameObject outfit = null;
        Transform target = null;
        switch (outfitType)
        {
            case OutfitType.Hair:
                outfit = Resources.Load<GameObject>($"Outfit/Hair/{outfitId}");
                target = hairSlot;
                if (hairSlot.childCount > 0)
                {
                    Destroy(hairSlot.GetChild(0).gameObject);
                }
                HairId = outfitId;
                break;
            case OutfitType.Cloth:
                outfit = Resources.Load<GameObject>($"Outfit/Clothes/{outfitId}");
                target = clothSlot;
                if (clothSlot.childCount > 0)
                {
                    Destroy(clothSlot.GetChild(0).gameObject);
                }
                ClothId = outfitId;
                break;
            case OutfitType.Pant:
                outfit = Resources.Load<GameObject>($"Outfit/Pants/{outfitId}");
                target = pantSlot;
                if (pantSlot.childCount > 0)
                {
                    Destroy(pantSlot.GetChild(0).gameObject);
                }
                PantId = outfitId;
                break;
            case OutfitType.Shoes:
                outfit = Resources.Load<GameObject>($"Outfit/Shoes/{outfitId}");
                target = shoesSlot;
                if (shoesSlot.childCount > 0)
                {
                    Destroy(shoesSlot.GetChild(0).gameObject);
                }
                ShoesId = outfitId;
                break;
        }
        var outfitObj = Instantiate(outfit, target);
        var smr = outfitObj.GetComponent<Outfit>().SkinnedMeshRenderer;
        var bones = SkinnedMeshHelper.GetNewBones(avatarSkinnedMesh, smr);
        smr.bones = bones;
    }
}
```
### ChangeOutfitController.cs
换装演示场景的UI控制逻辑。
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ChangeOutfitController : MonoBehaviour
{
    [SerializeField] private Button prevHair;
    [SerializeField] private Button nextHair;
    [SerializeField] private Button prevCloth;
    [SerializeField] private Button nextCloth;
    [SerializeField] private Button prevPant;
    [SerializeField] private Button nextPant;
    [SerializeField] private Button prevShoes;
    [SerializeField] private Button nextShoes;

    [SerializeField] private EquipmentManager equipmentMgr;

    private int currClothIndex = 1;
    private int currHairIndex = 1;
    private int currPantIndex = 1;
    private int currShoesIndex = 1;

    // Start is called before the first frame update
    void Start()
    {
        equipmentMgr.LoadEquipment();

        prevHair.onClick.AddListener(() => { ChangeOutfit(OutfitType.Hair, false); });
        nextHair.onClick.AddListener(() => { ChangeOutfit(OutfitType.Hair, true); });
        prevCloth.onClick.AddListener(() => { ChangeOutfit(OutfitType.Cloth, false); });
        nextCloth.onClick.AddListener(() => { ChangeOutfit(OutfitType.Cloth, true); });
        prevPant.onClick.AddListener(() => { ChangeOutfit(OutfitType.Pant, false); });
        nextPant.onClick.AddListener(() => { ChangeOutfit(OutfitType.Pant, true); });
        prevShoes.onClick.AddListener(() => { ChangeOutfit(OutfitType.Shoes, false); });
        nextShoes.onClick.AddListener(() => { ChangeOutfit(OutfitType.Shoes, true); });
    }

    private void ChangeOutfit(OutfitType outfitType, bool isNext)
    {
        switch (outfitType)
        {
            case OutfitType.Hair:
                if (isNext)
                {
                    currHairIndex = currHairIndex < 5 ? ++currHairIndex : 1;
                }
                else
                {
                    currHairIndex = currHairIndex > 1 ? --currHairIndex : 5;
                }
                equipmentMgr.ChangeOutfit(outfitType, currHairIndex);
                break;
            case OutfitType.Cloth:
                if (isNext)
                {
                    currClothIndex = currClothIndex < 5 ? ++currClothIndex : 1;
                }
                else
                {
                    currClothIndex = currClothIndex > 1 ? --currClothIndex : 5;
                }
                equipmentMgr.ChangeOutfit(outfitType, currClothIndex);
                break;
            case OutfitType.Pant:
                if (isNext)
                {
                    currPantIndex = currPantIndex < 5 ? ++currPantIndex : 1;
                }
                else
                {
                    currPantIndex = currPantIndex > 1 ? --currPantIndex : 5;
                }
                equipmentMgr.ChangeOutfit(outfitType, currPantIndex);
                break;
            case OutfitType.Shoes:
                if (isNext)
                {
                    currShoesIndex = currShoesIndex < 5 ? ++currShoesIndex : 1;
                }
                else
                {
                    currShoesIndex = currShoesIndex > 1 ? --currShoesIndex : 5;
                }
                equipmentMgr.ChangeOutfit(outfitType, currShoesIndex);
                break;
        }
    }
}
```
## 许可声明
请勿使用美术素材在任何形式的作品中，谢谢。
## 效果演示
<iframe width="560" height="315" src="https://www.youtube.com/embed/JdklEM-HwrI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>