# Understand Functions
1. [split](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#split)
2. [range](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#range)
3. [add](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#add)
4. [int](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#int)
5. [substring](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#substring)
6. [length](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#length)
7. refer more functions from [doc](https://learn.microsoft.com/en-us/azure/data-factory/control-flow-expression-language-functions#functions)
# Get Excel Number of Sheets Dynamically and load it.

![image](https://github.com/rritec/Cloud-Data-Engineering/assets/20516321/c927eed5-6b60-47a1-92a3-be4b37eb0576)


1. Get Metadata From Excel
    1. Create a pipeline with the name of **Get Excel Number of Sheets Dynamically and load it**
    2. Drag and Drop **Get Metadata** activity into pipeline workarea
    3. Click on **Settings** > Click on **Dataset** `+New` > Select **Azure Blob Storage** > Click on **Continue** > Select **Excel** > Click on **Continue**
    4. Provide Dataset name as **ds_sec_excel** > Select blob linked service
    5. browse the excel file
    6. Select **Index**
    7. Select **First Row as Header**
    8. Import Schema as **None**
    9. Click on **Advanced** > click on **Open Dataset**
    10. Click on **parameters** > Clcik on **+New** > Name it as **pSheetIndex** > Data type as **Int**
    11. Click on **Connections** > 
    12. Provide Sheet index expressionas 
        ``` json
        @dataset().pSheetIndex
        
        ```
    13. Provide parameter value as **999**
    14. Under **Field List** > Click on **New** > select **argument** as **Structure** 

3. Get Error of Metadata Activity

    1. Drag and drop **Set Variable** activity into the work area > Name it as **Get Error of Metadata activity**
    2. connect **Get Metadata** fail with **Set Variable**
    3. Click on **Settings** > Click on **New** > Add new variable name as **Get Error** and Type as **String** > Click on **Conform**
    4. Provide Value as


       ![image](https://user-images.githubusercontent.com/20516321/228446981-5b93b669-1f25-4cfe-953a-0ff5d61b45fd.png)


4. Create Range Object
    1. Drag and drop one more **Set Varaible** > Name it as **range_of_page_numbers**
    2. Connect Previous activity to this activity using **Success**
    3. Click on **Settings** > Click on **New** > Add new variable name as **Range of Page Numbers** and Type as **Array** > Click on **Conform**
    4. Provide Value as
        ```
            @range(
                    0,
                    add(
                        int(
                            substring(
                                variables('Get Error'),
                                3,
                                sub(
                                    length(
                                        variables('Get Error')
                                    ),
                                    4
                                )
                            )
                        )
                        ,1
                    )
                
                )
        ``` 

![image](https://user-images.githubusercontent.com/20516321/228446856-7526191b-6d80-4341-a50e-db328ab64525.png)
 
4. Create ForEach and Copy data activities
    1. Develop based on previous exercise knowledge
5. Run and observe each step input and ouput

![image](https://user-images.githubusercontent.com/20516321/228448475-57c9a1fd-520c-4c7d-b532-7b8430774e99.png)



