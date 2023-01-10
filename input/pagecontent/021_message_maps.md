このセクションでは、メッセージレベルのマッピングやデータ種別ごとの補足事項について説明する。

### メッセージレベルマッピング仕様
 * メッセージレベルのマッピングはSS-MIX2のデータ種別単位で定義する。個々のデータ種別に対応する内容はこのページの下部にある[データ種別一覧](#データ種別一覧)のリンク先で説明する。
ユースケースについては [ユースケース](040_usecases.html) を参照のこと。<br/>
 * メッセージレベルのマッピングでは HL7 V2 メッセージ を Bundle にマッピングすることを基本とするが、Bundle.type は問わない。ユースケースに応じた Bundle.type を指定し、その仕様に従いリソースを生成すること。<br/>
 * メッセージレベルのマッピングでは各リソース要素からの参照（例えばMedicationRequest.subject）へのマッピングについては割愛する。セグメントレベルのマッピングを参照のこと。
<br/><br/>

### データ種別一覧
#### ADT-00 患者基本情報
* [Message ADT_00 to Bundle](ConceptMap-message-adt-00-to-bundle.html) - ADT-00 患者基本情報<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment MSH to Provenance](ConceptMap-segment-msh-to-provenance.html) - 共通 MSH Provenance<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>
  - [Segment ADT_00_AL1 to AllergyIntolerance](ConceptMap-segment-adt-00-al1-to-allergyintolerance.html) - ADT-00 AL1  アレルギー情報<br/>
  - [Segment ADT_00_OBX to Observation](ConceptMap-segment-adt-00-obx-to-observation.html) - ADT-00 OBX  検査結果<br/>
  - [Segment ADT_00_IN1 to Coverage](ConceptMap-segment-adt-00-in1-to-coverage.html) - ADT-00 IN1  保険<br/><br/>

##### DB1セグメントの扱い
 * [http://hl7.org/fhir/R4/extension-patient-disability.html](http://hl7.org/fhir/R4/extension-patient-disability.html)を使用する。<br/>
<br/>

#### ADT-XX 入退院・転送・外出泊・帰院
* [Message ADT_22 to Bundle](ConceptMap-message-adt-22-to-bundle.html) - ADT-22 入院実施<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>

##### 適用対象となるデータ種別
 * ADT-21 入院予約<br/>
 * ADT-22 入院実施<br/>
 * ADT-31 外出泊実施<br/>
 * ADT-32 外出泊帰院実施<br/>
 * ADT-41 転科転棟予約<br/>
 * ADT-42 転科転棟実施<br/>
 * ADT-51 退院予約<br/>
 * ADT-52 退院実施<br/>

##### MSH-9.2（イベント型）・ status 間のマッピング
  - [Encounter.status](ConceptMap-table-v2-0003-to-encounterstatus.html)<br/>
  - [Encounter.location.status](ConceptMap-table-v2-0003-to-encounterlocationstatus.html) <br/>

##### 入退院日時および事象発生日時とリソース要素との関係
* EVN-6（事象発生日時）：ADT-31・ADT-32・ADT-41・ADT-42（転送・外出泊・帰院）で使用する。Encounter.location.period.start に設定する。<br/>
* PV1-44（入院日時）：Encounter.period.start に設定する。ADT-22 の場合、Encounter.location.period.start にも同様の値を設定する。<br/>
* PV1-45（退院日時）：Encounter.period.endに設定する。ADT-52 の場合、Encounter.location.period.end にも同様の値を設定する。<br/>
* PV2-8（予定入院日時）：ADT-51で使用する。Encounter.period.start に設定する。ADT-21 の場合、Encounter.location.period.startにも同様の値を設定する。<br/>
* PV2-9（予定退院日時）：ADT-21またはADT-51で使用する。Encounter.period.end に設定する。ADT-51 の場合、Encounter.location.period.end にも同様の値を設定する。<br/>
* PV2-47（予定帰院日時）：ADT-31で使用する。 Encounter.period.location.end に設定する。<br/>
<br/>

#### ADT-61 アレルギー情報
* [Message ADT_61 to Bundle](ConceptMap-message-adt-61-to-bundle.html) - ADT-61 アレルギー情報<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment ADT_61_IAM to AllergyIntolerance](ConceptMap-segment-adt-61-iam-to-allergyintolerance.html) - ADT-61 IAM アレルギー情報<br/>
<br/>

#### PPR-01 病名（歴）情報
* [Message PPR_01 to Bundle](ConceptMap-message-ppr-01-to-bundle.html) - PPR-01 病名（歴）情報<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment PPR_01_PRB to Condition](ConceptMap-segment-ppr-01-prb-to-condition.html) - PPR-01 PRB プロブレム詳細<br/>
<br/>

#### OMP-01 処方オーダ
* [Message OMP_01 to Bundle](ConceptMap-message-omp-01-to-bundle.html) - OMP-01 処方オーダ<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>
  - [Segment AL1 to AllergyIntolerance](ConceptMap-segment-adt-00-al1-to-allergyintolerance.html) -  AL1  アレルギー情報<br/>
  - [Segment OMP_01_RXE to MedicationRequest](ConceptMap-segment-omp-01-rxe-to-medicationrequest.html) - OMP-01 RXE 薬剤／処置 コード化されたオーダ<br/>
<br/>

#### OMP-12 注射実施
* [Message OMP_12 to Bundle](ConceptMap-message-omp-12-to-bundle.html) - OMP-12 注射実施<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment AL1 to AllergyIntolerance](ConceptMap-segment-adt-00-al1-to-allergyintolerance.html) -  AL1  アレルギー情報<br/>
  - [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>
  - [Segment OMP_12_RXA to MedicationAdministration](ConceptMap-segment-omp-12-rxa-to-medicationadministration.html) - OMP-12 RXA 薬剤／処置 投薬<br/>
  - [Segment OMP_12_RXA to Immunization](ConceptMap-segment-omp-12-rxa-to-immuznization.html) - OMP-12 RXA 薬剤／処置 投薬（検討中）<br/>

##### 注射種別
* OMP-12ではRXE-2に使用薬剤の種類の区分情報としての注射種別をセットする。取りうる値は「JHSI 表 0002-注射種別」を参照する。マッピングの際、注射種別を考慮する必要がある。<br/>
* 注射種別が「06 予防接種」の場合、RXAセグメントはImmunizationリソースにマッピングする必要がある（SHOULD）。<br/>
<br/>

#### OML-11 検体検査結果
* [Message OML_11 to Bundle](ConceptMap-message-oml-11-to-bundle.html) - OML-11 検体検査結果<br/>
  - [Segment MSH to Bundle](ConceptMap-segment-msh-to-bundle.html) - 共通 MSH メッセージヘッダ Bundle<br/>
  - [Segment MSH to MessageHeader](ConceptMap-segment-msh-to-messageheader.html) - 共通 MSH メッセージヘッダ MessageHeader<br/>
  - [Segment PID to Patient](ConceptMap-segment-pid-to-patient.html) - 共通 PID 患者識別<br/>
  - [Segment PV1 to Encounter](ConceptMap-segment-pv1-to-encounter.html) - 共通 PV1 来院情報<br/>
  - [Segment OML_11_SPM to Specimen](ConceptMap-segment-oml-11-spm-to-specimen.html) - OML-11 SPM 検体<br/>
  - [Segment OML_11_OBR to DiagnosticReport](ConceptMap-segment-oml-11-obr-to-diagnosticreport.html) - OML-11 OBR 検査要求<br/>
  - [Segment OML_11_ORC to ServiceRequest](ConceptMap-segment-oml-11-orc-to-servicerequest.html) - OML-11 ORC 共通オーダ<br/>
  - [Segment OML_11_OBX to Observation](ConceptMap-segment-oml-11-obx-to-observation.html) - OML-11 OBX 検査結果<br/>
<br/>

