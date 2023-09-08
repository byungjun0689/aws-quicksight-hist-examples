# QuickSight 실습

### 결과물
![Untitled](../img/lab/Untitled%201.png)

## 1. QuickSight 가입

- AWS QuickSight 서비스 가입 → Enterprise로 선택하여 가입 후 DataSet 연결 선택
- `DataSet → Athena 연결 시 에러 발생 → 권한이 없다.`

## 2. QuickSight → Athena 권한 부여

- **aws-quicksight-service-role-v0**
    - QuickSight 계정 생성 시 자동으로 생성되는 `Role`
- 권한 추가
    - `AWSQuicksightAthenaAccess`
- DataSets 추가 가능
    - Athena → Primary (Athena workgroup 선택) → Table 선택.
    
    ![Untitled](../img/lab/Untitled%202.png)
    

### 2.1 DataSets

- 목록
    - Products
    - dailybalju
    - dailytime
    - dailytimeproduct
    - dailytimesubcate

### 2.2 Products 생성

- `Edit/PreviewData` 선택 후 `Save & Publish` 선택.

![Untitled](../img/lab/Untitled%203.png)

![Untitled](../img/lab/Untitled%204.png)

### 2.3 나머지

- 나머지 4개 항목은 동일한 방법으로 수행.
- 결과

![Untitled](../img/lab/Untitled%205.png)

### 2.4 DataSet Date Type Change

- 현재 DataSet에 날짜 형태로 들어간 컬럼들은 다 String 형태로 되어있는데 DateType(yyyy-MM-dd)로 변경 필요.
- DataSet 선택 : dailybalju → `Edit DataSet`
    - balju_date : String → Date 로 변경.
        - yyyy-MM-dd
    - 변경 후 `Save & publish`
    
    ![Untitled](../img/lab/Untitled%206.png)
    
    ![Untitled](../img/lab/Untitled%207.png)
    

![Untitled](../img/lab/Untitled%208.png)

## 3. DataSet Join(선택)

- 이미 Glue를 통해서 데이터들끼리 Join은 이미 된 상태로 저장되어있지만 필요한 경우 Join을 수행할 수 있습니다.

### 3.1 dailytimetproduct 선택

- dataset 선택 후 우측 `Add Data` → `DataSets` → `products`
- product_cd 끼리 join 을 수행.

![Untitled](../img/lab/Untitled%209.png)

![Untitled](../img/lab/Untitled%2010.png)

![Untitled](../img/lab/Untitled%2011.png)

## 4. Analysis 생성.

### 4.1 New analysis → 신규 분석 생성

![Untitled](../img/lab/Untitled%2012.png)

### 4.2 Dataset 선택

- 기본이 될 Dataset 선택 후 해당 Analysis 안에 추가로 Dataset을 바인딩 할 수 있다.
- Dataset
    - `dailytime` : 일별 시간당 판매된 금액과 판매 카운팅 DataSet

![Untitled](../img/lab/Untitled%2013.png)

### 4.3 Theme 변경

- 원하는 테마색으로 선택.
- `Midnight` 으로 선택

![Untitled](../img/lab/Untitled%2014.png)

### 4.4 Dashboard Outline Size 설정

- `Settings` → `Free-Form` → 1600px(default) 또는 원하는 크기 선택 후 → `Apply` 적용.

![Untitled](../img/lab/Untitled%2015.png)

### 4.5 DataSets 추가

- 하나의 Dashboard에서 다양한 Dataset을 사용하고자 할 경우 DataSet추가를 통해 사용가능하다.
- Dataset → 연필 → 데이터셋 추가

![Untitled](../img/lab/Untitled%2016.png)

- `Add Dataset`
    - dailytimeproduct
    - dailytimesubcate
    - dailybalju
    - products

![Untitled](../img/lab/Untitled%2017.png)

![Untitled](../img/lab/Untitled%2018.png)

### 4.6 Parameter 생성

- Visualization을 수행하면서 필요할때마다 생성이 가능.
- 사전에 필요한 Parameter 미리 생성
    - `기간` : From, To
    - `분류` : 대분류, 중분류, 소분류, 제품
    
    ![Untitled](../img/lab/Untitled%2019.png)
    
- `**기간 : FromDate, ToDate**`
    - 동일한 방법으로 생성.
    
    ![Untitled](../img/lab/Untitled%2020.png)
    
    - Create → `Parameter Added` → `Control`
        - 생성한 파라미터를 어디에 사용할 것인지 미리 지정 가능.
    
    ![Untitled](../img/lab/Untitled%2021.png)
    
    - Name
        - FROM
    - Date Format
        - YYYY-MM-DD
    
    ![Untitled](../img/lab/Untitled%2022.png)
    
- `**분류**`
    - 대분류 : Division
        - DateType → String
        - Values → Multiple values
        
        ![Untitled](../img/lab/Untitled%2023.png)
        
        - `Apply → Parameter added → Add Control`
            - `Name` : 대분류
            - `Style` : Dropdown - multiselect
            - `Values` : Link to a dataset field
            - `Dataset` : products
            - `Field` : division_name
        
        ![Untitled](../img/lab/Untitled%2024.png)
        
    - 중분류 : MainCategory
        - 대분류와 Parameter 생성은 동일, Add Control 까지 수행
        - 하지만 대분류 선택 후 중분류의 목록이 변경 필요
            - 예) 가공식사제품 선택 → 중분류에서 대분류가 가공식사제품인 중분류만 표시 되도록 처리.
        
        ![Untitled](../img/lab/Untitled%2025.png)
        
        - Control → `Edit` Control → `Control Options` → `Show Relevant values Only`
            
            ![Untitled](../img/lab/Untitled%2026.png)
            
            ![Untitled](../img/lab/Untitled%2027.png)
            
            ![Untitled](../img/lab/Untitled%2028.png)
            
    - 소분류 : SubCategory, 제품 : Product
        - 중분류와 같은 방법으로 생성
        - 소분류 → 중분류
        - 제품 → 소분류
        
        ![Untitled](../img/lab/Untitled%2029.png)
        

## 5. 시각화

### 5.1 일별 매출 Text 표시

- `Insights` → Select a new fields → sum_sales(dailytime)
- Select Visualization → `Add Filter` → tr_date

![Untitled](../img/lab/Untitled%2030.png)

- Edit Filter
    - Condition : Equals
    - Check User Parameters
    - Date Parameter
        - ToDate(TO)
    - `Apply`(맨 하단)

![Untitled](../img/lab/Untitled%2031.png)

- Title Double Click → Editing

![Untitled](../img/lab/Untitled%2032.png)

- 조건에 따른 글자색 변경 및 ICON 추가
    - Visual → Condtitional formatting
    - Text Color, ICON 조건에 맞춰서 추가.

![Untitled](../img/lab/Untitled%2033.png)

![Untitled](../img/lab/Untitled%2034.png)

### 5.2 일자/대분류별 매출

**5.2.1 DataSet 변경** : `dailytrimesubcate`

![Untitled](../img/lab/Untitled%2035.png)

**5.2.2 좌측 상단** `+ADD` → Add Visual 선택

![Untitled](../img/lab/Untitled%2036.png)

**5.2.3 하단 차트 변경** : `Pivot Table`

![Untitled](../img/lab/Untitled%2037.png)

**5.2.4 사용할 컬럼 선택**

- Rows : division_name(대분류명)
- Valeus : sum_sales → 자동으로 `Sum`

![Untitled](../img/lab/Untitled%2038.png)

**6.2.5 일별 Filter 추가**

- `Visual` 선택 후 좌측 `Filter` 메뉴 선택. → `Add Filter` →`tr_date(컬럼)` 선택

![Untitled](../img/lab/Untitled%2039.png)

- 생성된 Filter → Edit  → Apply
    - Conditions : `Equals`
    - `Check Use Parameters`
    - Date Parameter → `ToDate`
        
        ![Untitled](../img/lab/Untitled%2040.png)
        
        ![Untitled](../img/lab/Untitled%2041.png)
        

**5.2.6 Title 변경**

- Sum of Sum_sales by Division_name 더블클릭 (기존 제목)
- 내용을 아래와 같이 변경 → Save
    - 일자/대분류별 매출 : ${ToDate}
        - ${ToDate} : 우측 Parameters 선택하면 됨
    
    ![Untitled](../img/lab/Untitled%2042.png)
    

5**.2.7 Table Column명 변경**

- Visual 선택 후 연필 모양 메뉴 선택
    
    ![Untitled](../img/lab/Untitled%2043.png)
    
    ![Untitled](../img/lab/Untitled%2044.png)
    
- Row Names : 대분류
- Value Names : 매출

**5.2.8 판매량 컬럼 추가**

- Visual 선택 → Data Set Field List 에서 count_sales 선택 → Values 로 Drag&Drop

![Untitled](../img/lab/Untitled%2045.png)

### 5.3 일자/제품별 매출

- `5**.2 일자/대분류별 매출과 동일하게 처리**`
- DataSet : `dailytimeproduct`
- Visual : `PivotTable`
- Rows : `product_name`
- Values
    - `sum_sales`
    - `count_sales`

![Untitled](../img/lab/Untitled%2046.png)

![Untitled](../img/lab/Untitled%2047.png)

### 5.4 기간별 매출

**5.4.1 Visual 선택**

- DataSet : `dailytime`
- Add Visual
    - `Vertical Bar Chart`
        
        ![Untitled](../img/lab/Untitled%2048.png)
        
- X-axis : `tr_date`
- Value : `sum_sales`

![Untitled](../img/lab/Untitled%2049.png)

**5.4.2 Filter 적용(기간)**

- Visual 선택 → `Add Filter` → Column 선택(`tr_date`) → Apply

![Untitled](../img/lab/Untitled%2050.png)

**5.4.3 Title 변경**

- 기간별 매출 : ${FromDate} ~ ${ToDate)

![Untitled](../img/lab/Untitled%2051.png)

**5.4.4. TEST** 

- `ToDate` → `2015-01-09`로 변경

![Untitled](../img/lab/Untitled%2052.png)

### 5.5 Donuts Chart (기간별 매출 비율)

- Donunts Chart를 통해 Drill Down 형태로 차트를 생성
- 대분류 → 중분류 → 소분류 → 제품별까지 Detail하게 Drill Down 될 수 있도록 처리.

**5.5.1 DataSet, Visual 선택**

- DataSet : `dailytimeproduct`
- Visual : Add Visual, `Donut Chart`

![Untitled](../img/lab/Untitled%2053.png)

**5.5.2 1 Layer 데이터 선택** 

- Group : `division_name`(대분류)
- Values : sum_sales

![Untitled](../img/lab/Untitled%2054.png)

![Untitled](../img/lab/Untitled%2055.png)

**5.5.3 [2,3,4] Layer 추가** 

- Group : maincategory_name, sub_category_name, product_name
- 선택한 컬럼 Drag & Drop
    - 주의 사항 : Drag 시 Drill down으로 하겠다고 표시가 나타날때 Drop을 수행.
        
        ![Untitled](../img/lab/Untitled%2056.png)
        
        ![Untitled](../img/lab/Untitled%2057.png)
        

**5.5.4 Drill Down 사용법** 

- 확인하고자하는 항목 우클릭 → Drill down to 세부컬럼명
    - ex) Drill down to maincategory_name

![Untitled](../img/lab/Untitled%2058.png)

![Untitled](../img/lab/Untitled%2059.png)

![Untitled](../img/lab/Untitled%2060.png)

- 상위로 올라가고 싶을때 : 동일방법
    - 우클릭 → Drill Up 상위컬럼
    
    ![Untitled](../img/lab/Untitled%2061.png)
    

**5.5.5 Filter 추가**

- 기간별 매출.
- Add Filter → 컬럼선택(`tr_date`)
- Edit
    - `Check Parameters`
    - StartDate : `FromDate`
    - EndDate : `ToDate`
    - `Check Include end date`
    
    ![Untitled](../img/lab/Untitled%2062.png)
    

## 6. 출력 및 대시보드 내보내기

### 6.1 출력 또는 PDF 생성

- 우측 상단 Export 메뉴 클릭
- PDF 또는 Print 선택

![Untitled](../img/lab/Untitled%2063.png)

### 6.2 대시보드 생성

- 우측 Share 버튼 클릭 → Publish dashboard 선택
    
    ![Untitled](../img/lab/Untitled%2064.png)
    
    ![Untitled](../img/lab/Untitled%2065.png)
    
    ![Untitled](../img/lab/Untitled%2066.png)
    

### 6.3 E-mail (Report Dashboard)

- 생성된 대시보드 내용을 메일로 전송
- 대시보드 → Share 메뉴 선택 → Email report 선택
    
    ![Untitled](../img/lab/Untitled%2067.png)
    
- 상세 조건 작성 후 전송
    
    ![Untitled](../img/lab/Untitled%2068.png)
    

![Untitled](../img/lab/Untitled%2069.png)