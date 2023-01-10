<!-- mappings.md {% comment %}
*****************************************************************************************
*
* Index of Mappings.
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

このセクションでは、このガイドで定義されているマッピングの種類を説明する。<br/>

### マッピング仕様
#### マッピングレベル

 * ガイドラインでは下表のとおり、マッピングの種類をメッセージレベル（Messages）、セグメントレベル（Segments）、データタイプレベル（DataTypes）、テーブル（用語）レベル（Tables）で定義する。<br/><br/>

| Lv | 名称 |  | 説明 |
| :--- | :--- | :--- |
| 0 | メッセージレベル | Messages | HL7 V2 メッセージ単位のマッピングを表す。マッピング内容はメッセージ配下のセグメントを変換前のデータ要素としたFHIRリソース要素との関係性である。 |
| 1 | セグメントレベル | Segments | HL7 V2 メッセージを構成するセグメント単位のマッピングを表す。マッピング内容はセグメント配下の要素を変換前のデータ要素としたFHIRリソース要素との関係性である。 |
| 2 | データタイプレベル | DataTypes | セグメントの各要素に割り当てられたデータ型単位のマッピングを表す。マッピング内容はデータタイプの要素（例えばCWE.1など）を変換前のデータ要素としたFHIRリソース要素との関係性である。 |
| 3 | テーブルレベル | Tables |SS-MIX2で使用するコード体系とFHIRのコード体系間のコードマッピングを表す。定義は FHIRプロファイルの Terminology Binding が強度で、SS-MIX2のコード体系をそのまま使用できない場合に限り作成する。 |

<br/>

各レベルの詳細はこのページの下部にある[マッピング一覧](#マッピング一覧)で説明する。<br/>
<br/>

#### ConceptMapリソース
 * マッピングを表現するために[ConceptMap]({{site.data.fhir.path}}conceptmap.html)リソースを用いる。<br/>
 * ガイドラインでは[StructureMap]({{site.data.fhir.path}}structuremap.html)リソースは定義・提供していない。現時点において、ConceptMap による高水準のマッピング仕様のみを定義する。<br/>
 * いずれのマッピングレベルにおいても A to B のマッピングを 1件の ConceptMap を用いて表現する（メッセージレベル1件もセグメントレベル1件も ConceptMap 1件である）。<br/>
<br/>

#### データ要素の表現
 * マッピング1つを[ConceptMap.group.element]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element)で表現する。<br/>
 * [ConceptMap.group.source]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.source)が変換前の体系である。<br/>
 * [ConceptMap.group.target]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.target)が変換後の体系である。<br/>
 * [ConceptMap.group.element.code]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element.code)が変換前のデータ要素やコードである。<br/>
 * [ConceptMap.group.element.target]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element.target)が変換後のデータ要素やコードを含む要素である。<br/>
 * メッセージレベルのデータ要素<br/>
   - メッセージレベルの変換前のデータ要素は「SS-MIX2標準化ストレージ仕様書」の「セグメント構成」における階層を半角ピリオドで結合し表現する。
   ルートは [MSH-9.1]_[MSH-9.2] とする。例えば 「ADT^A08」メッセージのPIDを表すのであれば「ADT_A08.PID」とする。<br/>
   - メッセージレベルの変換後のデータ要素はFHIRのリソースタイプまたはプロファイル名とする。<br/>
 * セグメントレベルのデータ要素<br/>
   - セグメントレベルの変換前のデータ要素は「SS-MIX2標準化ストレージ仕様書」の「セグメント定義」の1列目とし、セグメントと要素を表現する。データタイプで階層化されている場合、データタイプ上のインデックスを半角ピリオドで結合し表現してもよい。
 例えばPID-11の4番目の要素を表現する場合は「PID-11.4」とする。<br/>
   - セグメントレベルの変換後のデータ要素はFHIR要素のパスとする（リソースタイプをルートとする）。<br/>
 * データタイプレベルのデータ要素<br/>
   - データタイプレベルの変換前のデータ要素はデータタイプとデータタイプ上のインデックスを半角ピリオドで結合し表現する。例えばCWEの1番目の要素であれば「CWE.1」とする。<br/>
   - データタイプレベルの変換後のデータ要素はFHIR要素のパスとする。<br/>
 * テーブルレベルのデータ要素<br/>
   - データタイプレベルの変換前のデータ要素はコードとする。<br/>
   - データタイプレベルの変換後のデータ要素はコードとする。<br/>
<br/>

#### データ要素同士の関係性
 * マッピングにおける関係性を表すために [ConceptMap.group.element.target.equivalence]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element.target.equivalence) を用いる。<br/>
   - 変換前データと変換後データが等価（外延が等しい）である場合は [equivalent]({{site.data.fhir.path}}codesystem-concept-map-equivalence.html#concept-map-equivalence-equivalent) としている。<br/>
   - 変換前データと変換後データが関連しているで場合は [relatedto]({{site.data.fhir.path}}codesystem-concept-map-equivalence.html#concept-map-equivalence-relatedto) としている。<br/>
   - 変換前データがマッピングできない場合は [unmatched]({{site.data.fhir.path}}codesystem-concept-map-equivalence.html#concept-map-equivalence-unmatched) を設定する。<br/>
   - 変換前データを変換後データにマッピングしないことを明記する場合は [disjoint]({{site.data.fhir.path}}codesystem-concept-map-equivalence.html#concept-map-equivalence-disjoint) を設定する。<br/>
 * equivalence で表現できない特別な条件や注釈がある場合、[ConceptMap.group.element.target.comment]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element.target.comment) に表記する。<br/>
 * [ConceptMap.group.element.target.dependsOn]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group.element.target.dependsOn) は用いない。<br/><br/>

 * メッセージレベルのマッピングにおいて、1件のリソースが複数のセグメントで構成される場合がある。
 このような場合、カーディナリティに関わらず（例えば1件のRDE_O11には複数のRXEが含まれる可能性がある）、
 セグメントの種類分（下記の例においては5件）の ConceptMap.group.element を作成する。ただし、参照の場合（例えばMedicationRequest.subject）についてはガイドラインの記述をメッセージレベルでは省略する場合もある。<br/>
   - 例えば MedicationRequest は PID, ORC, RXE, TQ1, RXR からのマッピングであるため 5件の ConceptMap.group.element を作成する。<br/>
   <br/>
   
 * メッセージレベルのマッピングにおいて、1件のリソースを生成する場合、そのベースとなるセグメントを定義する。ベースとなるセグメントとFHIRリソースは外延的に等しくあるべきである。また、FHIRリソースに内包される要素をベースとなるセグメント以外から取得することができる。
 ベースとなるセグメントの関係性は equivalent とし、それ以外のセグメントの関係性は relatedto とする。<br/>
   - 例えば OMP-01（処方オーダ）において MedicationRequest（内服・外用薬剤処方） に対しマッピングを行う場合、 RXE（薬剤／処置 コード化されたオーダ） をベースとし、PID, ORC, TQ1, RXR の要素をセグメントレベルでマッピングする。
   この場合、RXE と MedicationRequest の 関係性は equivalent とし、内包される要素である PID, ORC, TQ1, RXR と MedicationRequest の 関係性は は relatedto とする。<br/>
   <br/>
   <img src="mapping.jpg" />
   <br/>
   <br/>

 * セグメントレベルのマッピングにおいて、セグメント と FHIRリソース は 1 対 1 になるとは限らず、1件のセグメントから複数のリソースに対するマッピングが発生する場合がある。
 このような場合、関係性は equivalent とし、FHIRリソース分（下記の例においては2件）の ConceptMap.group.element を作成する。<br/>
   - 例えば ORC-2 依頼者オーダ番号 は Observation.identifier と ServiceRequest.identifier にマッピングされる可能性がある。<br/>
   <br/>
 
 * セグメントレベルのマッピングにおいて、1件のリソースが複数のセグメント要素で構成される場合がある。このような場合、関係性は equivalent とし、セグメント要素分（下記の例においては2件）の ConceptMap.group.element を作成する。<br/>
   - 例えば MedicationRequest.medicationCodeableConcept は RXE-2 与薬コード からのマッピングであるが、 MedicationRequest.dosageInstruction.route は RXR-1 経路 からのマッピングである。<br/>
   <br/>
  
 * セグメントレベルのマッピングにおいて、複数件のセグメントが1件のリソースに集約される場合がある。このような場合、関係性は equivalent とし、セグメント要素分（下記の例においては2件）の ConceptMap.group.element を作成する。<br/>
   - 例えば PID-13 電話番号－自宅 と PID-14 電話番号－勤務先 は Patient.telecom の要素として設定される。<br/>
   <br/>
  
 * セグメントレベルのマッピングにおいて、あるセグメントがあるマッピングの結果に影響を及ぼすことがある。このような場合、関係性は relatedto とする。<br/>
   - 例えば OBX-2 値型 は Observation.value[x] のデータタイプに影響する。<br/>
   <br/>
  
  <br/>

### マッピング一覧

 * [Messages](021_message_maps.html) - メッセージレベルのマッピング
 * [Segments](022_segment_maps.html) - セグメントレベルのマッピング
 * [DataTypes](023_datatype_maps.html) - データタイプのマッピング
 * [Tables](024_table_maps.html) - テーブルレベルのマッピング

<br/>

