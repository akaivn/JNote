## POI

简介：

==Java操作Excel表格的API工具 ：Apache POI 和 Alibaba EasyExcel==

### 代码示例

准备环境，导入依赖！

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>

    <!--03 xls-->
    <!-- https://mvnrepository.com/artifact/org.apache.poi/poi -->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>4.1.2</version>
    </dependency>

    <!--07 xlsx-->
    <!-- https://mvnrepository.com/artifact/org.apache.poi/poi-ooxml -->
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>4.1.2</version>
    </dependency>

    <!--joda 时间转换工具类-->
    <!-- https://mvnrepository.com/artifact/joda-time/joda-time -->
    <dependency>
        <groupId>joda-time</groupId>
        <artifactId>joda-time</artifactId>
        <version>2.10.6</version>
    </dependency>
</dependencies>
```

#### Write

##### 1、常规写

```java
@Test
public void hSSFWrite() throws IOException {
    long begin = System.currentTimeMillis();
    // 创建表 03
    Workbook workbook = new HSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();
    // 创建行
    Row row = sheet.createRow(0);
    Cell cell = row.createCell(0);
    cell.setCellValue("今日新增观众");
    Cell cell2 = row.createCell(1);
    cell2.setCellValue("今日时间");

    Row row1 = sheet.createRow(1);
    row1.createCell(0).setCellValue("卢本伟");
    String time = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
    row1.createCell(1).setCellValue(time);

    // 生成路径
    String path = "D:\\";
    FileOutputStream fileOutputStream = new FileOutputStream(path + "文件名.xls");
    // 写入数据
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("写入完毕！时间为：" + (double)(end - begin) / 1000);
}
@Test
public void xSSFWrite() throws IOException {
    long begin = System.currentTimeMillis();
    // 创建表 03
    Workbook workbook = new XSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();
    // 创建行
    Row row = sheet.createRow(0);
    Cell cell = row.createCell(0);
    cell.setCellValue("今日新增观众");
    Cell cell2 = row.createCell(1);
    cell2.setCellValue("今日时间");

    Row row1 = sheet.createRow(1);
    row1.createCell(0).setCellValue("卢本伟");
    String time = new DateTime().toString("yyyy-MM-dd HH:mm:ss");
    row1.createCell(1).setCellValue(time);

    // 生成路径
    String path = "D:\\";
    FileOutputStream fileOutputStream = new FileOutputStream(path + "07.xlsx");
    // 写入数据
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("写入完毕！时间为：" + (double)(end - begin) / 1000);
}
```

##### 2、大数据写法

```java
@Test
/**
     * 特点：速度快，缺点就是最多只支持65535行数据录入
     */
public void hSSFWriteBigData() throws IOException {
    long begin = System.currentTimeMillis();
    // 创建表 03
    Workbook workbook = new HSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();

    for (int rowNum = 0; rowNum < 65535; rowNum++) {
        // 创建行
        Row row = sheet.createRow(rowNum);
        for (int columNum = 0; columNum < 11; columNum++) {
            // 创建列  放数据
            row.createCell(columNum).setCellValue(columNum);
        }
    }

    // 生成路径
    String path = "D:\\";
    FileOutputStream fileOutputStream = new FileOutputStream(path + "03BigData.xls");
    // 写入数据
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("写入完毕！时间为：" + (double)(end - begin) / 1000);
}
@Test
/**
     * 特点：速度慢，支持上百万条数据录入，数据量大时出现 java.lang.OutOfMemoryError  OOM
     */
public void xSSFWriteBigData() throws IOException {
    long begin = System.currentTimeMillis();
    // 创建表 03
    Workbook workbook = new XSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();

    for (int rowNum = 0; rowNum < 100000; rowNum++) {
        // 创建行
        Row row = sheet.createRow(rowNum);
        for (int columNum = 0; columNum < 11; columNum++) {
            // 创建列  放数据
            row.createCell(columNum).setCellValue(columNum);
        }
    }

    // 生成路径
    String path = "D:\\";
    FileOutputStream fileOutputStream = new FileOutputStream(path + "07BigData.xlsx");
    // 写入数据
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("写入完毕！时间为：" + (double)(end - begin) / 1000);
}
```

##### 3、07大数据升级写法

```java
/**
     * 特点 ：快速提升写入时间，占用内存较多，时间换空间
     * @throws IOException
     */
@Test
public void sXSSFWrite() throws IOException {
    long begin = System.currentTimeMillis();
    // 创建表 03
    Workbook workbook = new SXSSFWorkbook();
    // 创建表
    Sheet sheet = workbook.createSheet();

    for (int rowNum = 0; rowNum < 100000; rowNum++) {
        // 创建行
        Row row = sheet.createRow(rowNum);
        for (int columNum = 0; columNum < 11; columNum++) {
            // 创建列  放数据
            row.createCell(columNum).setCellValue(columNum);
        }
    }

    // 生成路径
    String path = "D:\\";
    FileOutputStream fileOutputStream = new FileOutputStream(path + "sxssfBigData.xlsx");
    // 写入数据
    workbook.write(fileOutputStream);
    // 关闭流
    fileOutputStream.close();
    long end = System.currentTimeMillis();
    System.out.println("写入完毕！时间为：" + (double)(end - begin) / 1000);
}
```

#### Read

##### 1、07常规读，03需要置换对象

```java
@Test
public void hSSFRead() throws IOException {
    FileInputStream fileInputStream = new FileInputStream("D:\\sxssfBigData.xlsx");


    Workbook workbook = new XSSFWorkbook(fileInputStream);
    // 链式调用
    //        System.out.println(workbook.getSheet("Sheet0").getRow(1).getCell(1).getNumericCellValue());

    // for循环
    Sheet sheet0 = workbook.getSheet("Sheet0");
    for (int row = 0; row < 100; row++) {
        Row row1 = sheet0.getRow(row);
        for (int colum = 0; colum < 10; colum++) {
            System.out.print((int)row1.getCell(colum).getNumericCellValue()+ "\t");
        }
        System.out.println();
    }

    fileInputStream.close();
}
```

##### 2、类型不确定的读，以及公式读

###### 1、先读表头第一行数据

```java
ileInputStream fileInputStream = new FileInputStream(path + "数据表07.xlsx");
Workbook workbook=new XSSFWorkbook(fileInputStream);
Sheet sheet = workbook.getSheetAt(0);
Row rowTitle = sheet.getRow(0);
if(rowTitle!=null){
    int cellCount=rowTitle.getPhysicalNumberOfCells();    //拿到第row行的那一行的总个数
    for (int i = 0; i <cellCount ; i++) {  //循环个数取出
        Cell cell = rowTitle.getCell(i);
        if(cell!=null){          //如果不等于空取出值
            int cellType = cell.getCellType();   //这里是知道我们标题是String，考虑不确定的时候怎么取
            String cellValue = cell.getStringCellValue();
            System.out.print(cellValue+"|");
        }
    }
    System.out.println();
}
```

###### 2、类型准换，精准判断类型是什么，并且取出

```java
//获取表中内容
int rowCount=sheet.getPhysicalNumberOfRows();
for(int rowNum=1;rowNum<rowCount;rowNum++){
    Row rowData=sheet.getRow(rowNum); //取出对应的行
    if(rowData!=null){
        int cellCount=rowTitle.getPhysicalNumberOfCells();
        for(int cellNum=0;cellNum<cellCount;cellNum++){
            System.out.print("["+(rowNum+1+"-"+(cellNum+1)+"]"));
            Cell cell = rowData.getCell(cellNum);
            //匹配数据类型
            if(cell!=null){
                int cellType=cell.getCellType();
                switch (cellType){
                    case XSSFCell.CELL_TYPE_STRING: System.out.print("字符串："+cell.getStringCellValue());break;
                    case XSSFCell.CELL_TYPE_BOOLEAN: System.out.print("布尔："+cell.getBooleanCellValue());break;
                    case XSSFCell.CELL_TYPE_NUMERIC:
                        if(HSSFDateUtil.isCellDateFormatted(cell)){
                            System.out.println("日期格式："+new DateTime(cell.getDateCellValue()).toString("yyyy-MM-dd HH:mm:ss"));break;
                        }else
                            cell.setCellType(XSSFCell.CELL_TYPE_STRING);
                        System.out.print("整形："+cell.toString());break;
                    case XSSFCell.CELL_TYPE_BLANK: System.out.print("空");break;
                    case XSSFCell.CELL_TYPE_ERROR: System.out.print("数据类型错误");break;
                    case Cell.CELL_TYPE_FORMULA:
                        String formula=cell.getCellFormula();
                        System.out.println("公式:"+formula);

                        //
                        CellValue evaluate = formulaEvaluator.evaluate(cell);
                        String cellValue=evaluate.formatAsString();
                        System.out.println(cellValue);
                        break;
                    default:break;
                }
            }
        }
    }
}


fileInputStream.close();
```

## EasyExcel

官方文档：https://www.yuque.com/easyexcel/doc/easyexcel

1、导入依赖

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/easyexcel -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>2.2.6</version>
</dependency>
```



