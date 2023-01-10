このセクションでは、セグメントレベルのマッピングを説明する。

### メッセージレベルマッピング仕様
 * セグメントレベルのマッピングはSS-MIX2の各データ種別配下のセグメント単位で定義する。
個々のセグメントに対応する内容はこのページの下部にある[セグメント一覧](#セグメント一覧)のリンク先で説明する。
ユースケースについては [ユースケース](040_usecases.html) を参照のこと。<br/>
 * SS-MIX仕様書において、「JAHIS」項目オプション指定が「対象のトリガイベントでは使用されない（X）」「通常使用しない（N）」ものは設定（ [ConceptMap.group]({{site.data.fhir.path}}conceptmap-definitions.html#ConceptMap.group) ）に含めない。<br/>
 * マッピング定義は複数のセグメントを Source Code として含む場合がある。<br/>
 * Bundle-message にマッピングする場合、MessageHeader.focus に Bundle.entryに格納したリソースの参照を指定すること。<br/>
<br/><br/>

### セグメント一覧
#### 共通項目
* [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
* [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
* [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
* [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>
<br/>

#### ADT-00 患者基本情報
* [Segment ADT_00_AL1 to AllergyIntolerance](ConceptMap-segment-adt-00-al1-to-allergyintolerance.html) - ADT-00 AL1  アレルギー情報<br/>
* [Segment ADT_00_OBX to Observation](ConceptMap-segment-adt-00-obx-to-observation.html) - ADT-00 OBX  検査結果<br/>
* [Segment ADT_00_IN1 to Coverage](ConceptMap-segment-adt-00-in1-to-coverage.html) - ADT-00 IN1  保険<br/>
<br/>

#### ADT-12 外来診察の受付
* [Segment ADT_XX_PV1 to Encounter](ConceptMap-segment-adt-xx-pv1-to-encounter.html) - ADT-12 PV1 来院情報（検討中）<br/>
<br/>

#### ADT-XX 入退院・転送・外出泊・帰院
* [Segment ADT_XX_PV1 to Encounter](ConceptMap-segment-adt-xx-pv1-to-encounter.html) - ADT PV1 来院情報<br/>
<br/>

#### ADT-61 アレルギー情報
* [Segment ADT_61_IAM to AllergyIntolerance](ConceptMap-segment-adt-61-iam-to-allergyintolerance.html) - ADT-61 IAM アレルギー情報<br/>
<br/>

#### PPR-01 病名（歴）情報
* [Segment PPR_01_PRB to Condition](ConceptMap-segment-ppr-01-prb-to-condition.html) - PPR-01 PRB プロブレム詳細<br/>
<br/>

#### OMP-01 処方オーダ
* [Segment OMP_01_RXE to MedicationRequest](ConceptMap-segment-omp-01-rxe-to-medicationrequest.html) - OMP-01 RXE 薬剤／処置 コード化されたオーダ<br/>
<br/>

#### OMP-12 注射実施
* [Segment OMP_12_RXA to MedicationAdministration](ConceptMap-segment-omp-12-rxa-to-medicationadministration.html) - OMP-12 RXA 薬剤／処置 投薬<br/>
OMP-12 RXA 薬剤／処置 投薬（予防接種）
<br/>

#### OML-11 検体検査結果
* [Segment OML_11_SPM to Specimen](ConceptMap-segment-oml-11-spm-to-specimen.html) - OML-11 SPM 検体<br/>
* [Segment OML_11_OBR to DiagnosticReport](ConceptMap-segment-oml-11-obr-to-diagnosticreport.html) - OML-11 OBR 検査要求<br/>
* [Segment OML_11_ORC to ServiceRequest](ConceptMap-segment-oml-11-orc-to-servicerequest.html) - OML-11 ORC 共通オーダ<br/>
* [Segment OML_11_OBX to Observation](ConceptMap-segment-oml-11-obx-to-observation.html) - OML-11 OBX 検査結果<br/>
<br/>

