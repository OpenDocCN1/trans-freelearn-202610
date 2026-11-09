7. Next, we need to build a map that will transform the **BAPI_MATERIAL_GET_DETAILResponse** into a **MaterialDetailResponse** that we can send back to our calling CRM system. The map to do this is called **MaterialDetailResponseSAP_to_MaterialDetailResponseDisk.btm**. The nodes that are being mapped are as follows:

| BAPI_MATERIAL_GET_DETAILResponse | MaterialDetailResponse |
| /MATERIAL_GENERAL_DATA/MATL_DESC | MaterialDescription |
| /MATERIAL_GENERAL_DATA/MATL_TYPE | MaterialType |
| /MATERIAL_GENERAL_DATA/BASE_UOM | UnitOfMeasure |
| /MATERIAL_GENERAL_DATA/NET_WEIGHT | NetWeight |
| /MATERIAL_GENERAL_DATA/CREATED_ON | MaterialCreateDate |
| /MATERIAL_GENERAL_DATA/LAST_CHNGE | MaterialLastUpdated |
| /MATERIALVALUATIONDATA/MOVING_PR | MovingPrice |
| /MATERIALVALUATIONDATA/PRICE_UNIT | PriceUnit |
| /MATERIALVALUATIONDATA/CURRENCY | Currency |

![](bbox=[0.26, 0.44, 0.74, 0.79])