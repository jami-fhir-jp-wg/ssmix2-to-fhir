<!-- usecases.md {% comment %}
*****************************************************************************************
*
* UseCases.
*
*****************************************************************************************
{% endcomment %} -->

<style type="text/css">
table {
    border-collapse: collapse;
}
table, th, td {
    border: 1px solid gray;
}
</style>

このセクションでは、このガイドで定義されているマッピングのユースケースを説明する。<br/>

### 標準化ストレージをBundle-transactionに変換し、FHIRサーバに転送する
* このケースではメッセージ単位で Bundle-transaction を生成し、transactionインタラクションを用い転送する。<br/>
* このケースではBundle.entryに格納された、個別のリソースが生成される。<br/>
* メッセージから生成されるすべてのリソースをBundleに含めなくてもよい。<br/>
例えばOMP-01に含まれるPIDセグメントから生成されるPatientの実態は「ADT-00」であるため、別のトランザクションでPatientリソースが生成されているとしてもよい。
ただし、参照整合性は確保されなくてはならない。<br/>
<br/>

#### MessageHeaderリソース
* Bundle-transaction の場合は必須ではないが MessageHeaderリソースの追加してもよい。<br/>
* MessageHeader.eventCodingにはMSH-9.2の[HL70003 事象型](http://terminology.hl7.org/CodeSystem/v2-0003)を設定する。<br/>
* MessageHeader.reason に [HL70119 オーダ制御コード](http://terminology.hl7.org/CodeSystem/v2-0119) から処理区分に対応した値を設定する。オーダ制御の設定方法は次節を参考のこと。<br/>

* MessageHeaderの設定（処方オーダ）<br/>
    <font style="color: blue;">
      {<br/>
        　"resourceType": "MessageHeader",<br/>
        　"id": "1000094569.20160930.OMP-01.169300891137600.MSH",<br/>
        　"eventCoding": {<br/>
          　　"system": "http://terminology.hl7.org/CodeSystem/v2-0003",<br/>
          　　"code": "O11"<br/>
        　},<br/>
        　"source": {<br/>
          　　"name": "ssmix2tofhir",<br/>
          　　"endpoint": "http://ssmix2tofhir.exp.org"<br/>
        　},<br/>
        　"reason": {<br/>
          　"coding": [<br/>
            　{<br/>
              　　"system": "http://terminology.hl7.org/CodeSystem/v2-0119",<br/>
              　　"code": "NW"<br/>
            　}<br/>
          　]<br/>
        　},<br/>
        　"focus": [ <br/>
          　　{ <br/>
          　　　"reference": "MedicationRequest/0000000123.20220101.OMP-01.000000000012345.5" <br/>
          　　　"display": "0000000123.20220101.OMP-01.000000000012345.5.20220101235959568" <br/>
          　　}, <br/>
          　　{ <br/>
          　　　"reference": "MedicationRequest/0000000123.20220101.OMP-01.000000000012345.9" <br/>
          　　　"display": "0000000123.20220101.OMP-01.000000000012345.9.20220101235959568" <br/>
          　　} <br/>
        　] <br/>
      }<br/>
    </font>
    
<br/>
<br/>

#### コンディションフラグとインタラクション
* このケースにおいて、インタラクションの選択にはファイル名の「コンディションフラグ」を用いる。<br/>
下表でファイル名のコンディションフラグとインタラクションの関係について説明する。<br/>

| コンディションフラグ | FHIR インタラクション | method | オーダ制御コード | 備考 |
| :--- | :--- | :--- | :--- | :--- |
| 0 | delete | DELETE | CA | 削除 |
| 1 | create または update | POST または PUT | NW | 挿入 |
| 2 | delete | DELETE | CA | 削除 |

<br/>

#### メッセージ単位のデータ取得
 * transactionインタラクションにより個別リソースを作成した場合、 event, _lastUpdated 検索パラメータを指定し、
 MessageHeaderリソースを取得することで、MessageHeader.focus から関連リソースの参照の取得を想定する。<br/>
   - O11(処方オーダ) イベントを指定して、更新データを取得する。<br/>
     http://localhost/fhir-jpaserver/fhir/MessageHeader?event=O11&_lastUpdated=gt2022-03-01T09:59:43.758&_sort=_lastUpdated<br/>
     
   - MessageHeader.focusの設定<br/>
    新規（INS）・新規（INS）で更新を行い、MessageHeader の内容が変わらない場合、サーバ側で_lastUpdatedが更新されない場合があるため、クライアント側は実装を考慮する必要がある。<br/>
    これに対応するため、focus.displayに「[ロジカルID].[SS-MIXヘッダーのトランザクション日時]」などを設定しサーバ側で_lastUpdatedが更新されるようにしてもよい。<br/>
    <br/>
    <font style="color: blue;">
    "focus": [ <br/>
          　{ <br/>
          　　"reference": "MedicationRequest/0000000123.20220101.OMP-01.000000000012345.5" <br/>
          　　"display": "0000000123.20220101.OMP-01.000000000012345.5.20220101235959568" <br/>
          　}, <br/>
          　{ <br/>
          　　"reference": "MedicationRequest/0000000123.20220101.OMP-01.000000000012345.9" <br/>
          　　"display": "0000000123.20220101.OMP-01.000000000012345.9.20220101235959568" <br/>
          　} <br/>
        ] <br/>
    </font>
 <br/>

<br/>

### 標準化ストレージををBundle-messageに変換し、転送する
* このケースではトランザクションストレージファイル内のメッセージをBundle-messageに変換し、FHIRサーバなどに転送する（このケースでは転送方法は問わない）ことを想定している。<br/>
* 転送にFHIRインタラクションを用いる場合は update インタラクションで送信することを想定する。create-updateがサポートされていないはcreateを想定する。<br/>
* このケースではメッセージから生成されるすべてのリソースをBundleに含めることを推奨する。<br/>
* このケースではデータ取得側が Bundle-message 単位でデータを取得することを想定する。<br/>
* $process-message への対応についても検討する。<br/>
<br/>
<br/>

### トランザクションストレージをBundleに変換し、転送する
* このケースでは標準化ストレージに格納されている、メッセージをFHIRサーバなどに転送することを想定している。<br/>
* 基本的にトランザクションストレージと同じであるが、ロジカルIDの取得元やインタラクションの決定方法が異なる。<br/>
<br/>

#### SS-MIXヘッダとインタラクション
* このケースにおいて、インタラクション（実際はBundle.entry.request.method）の選択にはSS-MIXヘッダの「処理区分」を用いる。<br/>
* MessageHeaderの削除をPUTで送信してもよい。その場合、MessageHeader.focus を設定しない。
* このケースではデータ取得側が MessageHeader.focus から「有効なリソース」を取得することを想定する。<br/>
* 下表で処理区分とインタラクションの関係について説明する。<br/>

| 処理区分 | FHIR インタラクション | method | オーダ制御コード | 備考 |
| :--- | :--- | :--- | :--- | :--- |
| INS | create または update | POST または PUT | NW | 挿入 |
| DEL | update または delete | PUT または DELETE | CA | 削除、MessageHeaderの場合、methodをPUTとする |

<br/>

### インデックスデータベース（INDEXDB）を使用する
* 「トランザクション日時」により期間を指定しするなどしてインデックスデータベースから定期的に転送対象となるメッセージを取得し、FHIRサーバに転送することを想定している。<br/>
* 転送のプロセスは「標準化ストレージからFHIRサーバにリソースを転送する」ケースと同様である。<br/>
<br/>

### データ転送例
#### 転送例（bundle-trunsaction）
このケースではMessageHeader.focus から「有効なリソース」を取得することを想定する。<br/>
 * ADT-00（患者基本情報）を削除・新規で送信する 
   - 削除送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,-,ADT-00,999999999999999,DEL,-,20220101235959567<br/>
      - インタラクション: transaction
      - 送信対象リソース<br/>
        MessageHeader（ID: 0000000123.-.ADT-00.999999999999999.MSH, method: PUT, reason=CA, focusなし）<br/>
        Patient（ID: 0000000123.ADT-00, method: DELETE）<br/>
        ※ MessageHeader.focusを除外し送信、配下のリソースはDELETE, Encounter, AllergyIntolerance, Observation(検体検査結果)は除外し送信<br/>

   - 新規送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,-,ADT-00,999999999999999,INS,-,20220101235959568<br/>
      - インタラクション: transaction
      - 送信対象リソース<br/>
        MessageHeader（ID: 0000000123.-.ADT-00.999999999999999.MSH, method: PUT, reason=NW）<br/>
        Patient（ID: 0000000123.ADT-00, method: PUT）<br/>
        Observation（身長を入力, ID: 0000000123.-.ADT-00.999999999999999.7, method: PUT）<br/>
        Observation（体重を入力, ID: 0000000123.-.ADT-00.999999999999999.8, method: PUT）<br/>
        ※ Encounter, AllergyIntolerance, Observation(検体検査結果)は除外し送信<br/>
<br/>

 * OMP-01（処方オーダ）を削除・新規で送信する、新規でRXEを1件削除したケース  
   - 削除送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,20220101,OMP-01,000000000012345,DEL,-,20220101235959567<br/>
      - インタラクション: transaction
      - 送信対象リソース（Bundle-transaction entry配下のリソース）<br/>
        MessageHeader（ID: 0000000123.-.ADT-00.999999999999999.MSH, method: PUT, reason=CA）<br/>
        MedicationRequest（ID: 0000000123.20220101.OMP-01.000000000012345.5, method: DELETE, subject=0000000123.ADT-00）<br/>
        MedicationRequest（ID: 0000000123.20220101.OMP-01.000000000012345.9, method: DELETE, subject=0000000123.ADT-00）<br/>
        ※ Patient, Encounter は除外し送信<br/>

   - 新規送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,20220101,OMP-01,000000000012345,INS,-,20220101235959568<br/>
      - インタラクション: transaction
      - 送信対象リソース（Bundle-transaction entry配下のリソース）<br/>
        MessageHeader（ID: 0000000123.-.ADT-00.999999999999999.MSH, method: PUT, reason=NW）<br/>
        MedicationRequest（ID: 0000000123.20220101.OMP-01.000000000012345.5, method: PUT, subject=0000000123.ADT-00）<br/>
        ※ Patient, Encounter は除外し送信<br/>
<br/>

#### 転送例（bundle-message）
 * OMP-01（処方オーダ）の削除・新規で送信する 
   - 削除送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,20220101,OMP-01,000000000012345,DEL,-,20220101235959567<br/>
      - インタラクション: update
      - 送信対象リソース（Bundle-message）<br/>
        Bundle（ID: 0000000123,20220101,OMP-01,000000000012345, method: PUT）<br/>
        配下の MessageHeader（ID: 0000000123.20220101.OMP-01.000000000012345.MSH, reason=CA）<br/>

   - 新規送信
      - ヘッダー
        #SSMIX,2.00,9999999999,0000000123,20220101,OMP-01,000000000012345,INS,-,20220101235959568<br/>
      - インタラクション: update
      - 送信対象リソース（Bundle-message）<br/>
        Bundle（ID: 0000000123,20220101,OMP-01,000000000012345, method: PUT）<br/>
        配下の MessageHeader（ID: 0000000123.20220101.OMP-01.000000000012345.MSH, reason=NW）<br/>
<br/>

<br/>

