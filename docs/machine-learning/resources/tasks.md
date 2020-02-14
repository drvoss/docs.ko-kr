---
title: 기계 학습 작업
description: ML.NET에서 지원되는 다양한 기계 학습 작업 및 관련 작업을 살펴봅니다.
ms.date: 12/23/2019
ms.openlocfilehash: 6cd41065e668375537b9816ef7a208a65e0a523b
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745106"
---
# <a name="machine-learning-tasks-in-mlnet"></a>ML.NET의 기계 학습 작업

기계 학습 작업은 요청되는 문제나 질문 및 사용 가능한 데이터에 따라 수행되는 예측 또는 추론의 종류입니다. 예를 들어 분류 작업은 데이터를 범주에 할당하고, 클러스터링 작업은 유사성에 따라 데이터를 그룹화합니다.

기계 학습 작업은 명시적으로 프로그래밍되는 것이 아니라 데이터 패턴을 사용합니다.

이 문서에서는 사용자가 ML.NET에서 선택할 수 있는 다양한 기계 학습 작업과 몇 가지 일반적인 사용 사례에 대해 설명합니다.

시나리오에 적합한 작업을 선택했으면 모델을 학습할 최상의 알고리즘을 선택해야 합니다. 사용 가능한 알고리즘은 각 작업의 섹션에 나열됩니다.

## <a name="binary-classification"></a>이진 분류

두 클래스(범주) 중에 데이터의 인스턴스가 속한 클래스를 예측하는 데 사용되는 [감독된 기계 학습](glossary.md#supervised-machine-learning) 작업입니다. 분류 알고리즘의 입력은 레이블이 지정된 예제 집합입니다. 여기서 각 레이블은 0 또는 1의 정수입니다. 이진 분류 알고리즘의 출력은 분류자로, 레이블이 없는 새 인스턴스의 클래스를 예측하는 데 사용할 수 있습니다. 이진 분류 시나리오의 예는 다음과 같습니다.

* “긍정적” 또는 “부정적”으로 [Twitter 댓글의 감정 이해](../tutorials/sentiment-analysis.md).
* 환자에게 특정 질병이 있는지 여부 진단.
* 전자 메일을 “스팸”으로 표시할지 여부 결정.
* 사진이 특정 항목을 포함하는지 여부 확인(예: 개 또는 과일).

자세한 내용은 Wikipedia에서 [Binary classification](https://en.wikipedia.org/wiki/Binary_classification)(이진 분류) 문서를 참조하세요.

### <a name="binary-classification-trainers"></a>이진 분류 트레이너

다음 알고리즘을 사용하여 이진 분류 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>
* <xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>
* <xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>
* <xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>
* <xref:Microsoft.ML.Trainers.PriorTrainer>
* <xref:Microsoft.ML.Trainers.LinearSvmTrainer>

### <a name="binary-classification-inputs-and-outputs"></a>이진 분류 입력 및 출력

이진 분류에서 최상의 결과를 내려면 학습 데이터의 균형이 이루어져야 합니다(즉 긍정 및 부정 학습 데이터 수 일치). 누락 값은 학습 전에 처리되어야 합니다.

입력 레이블 열 데이터는 <xref:System.Boolean>이어야 합니다.
입력 기능 열 데이터는 고정 크기의 <xref:System.Single> 벡터여야 합니다.

이러한 트레이너는 다음 열을 출력합니다.

| 출력 열 이름 | 열 유형 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 모델에서 계산된 원시 점수|
| `PredictedLabel` | <xref:System.Boolean> | 점수 부호에 따라 예측된 레이블 음수 점수는 `false`에 양수 점수는 `true`에 매핑됩니다.|

## <a name="multiclass-classification"></a>다중 클래스 분류

데이터 인스턴스의 클래스(범주)를 예측하는 데 사용되는 [감독된 기계 학습](glossary.md#supervised-machine-learning) 작업입니다. 분류 알고리즘의 입력은 레이블이 지정된 예제 집합입니다. 각 레이블은 일반적으로 텍스트로 시작됩니다. 그런 다음, TermTransform을 통해 실행되어 키(숫자) 유형으로 변환됩니다. 분류 알고리즘의 출력은 분류자로, 레이블이 없는 새 인스턴스의 클래스를 예측하는 데 사용할 수 있습니다. 다중 클래스 분류 시나리오의 예는 다음과 같습니다.

* 개의 품종을 “시베리안 허스키”, “골든 리트리버”, “푸들” 등으로 결정.
* 동영상 리뷰를 “긍정적”, “중립” 또는 “부정적”으로 이해.
* 호텔 리뷰를 “위치”, “가격”, “청결도” 등으로 범주화.

자세한 내용은 Wikipedia에서 [Multiclass classification](https://en.wikipedia.org/wiki/Multiclass_classification)(다중 클래스 분류) 문서를 참조하세요.

>[!NOTE]
>일 대 다(One vs all)는 모든 [이진 분류 학습자](#binary-classification)를 다중 클래스 데이터 세트에서 작동하도록 업그레이드합니다. 자세한 내용은 [Wikipedia](https://en.wikipedia.org/wiki/Multiclass_classification#One-vs.-rest) )를 참조하세요.

### <a name="multiclass-classification-trainers"></a>다중 클래스 분류 트레이너

다음 학습 알고리즘을 사용하여 다중 클래스 분류 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>
* <xref:Microsoft.ML.Trainers.OneVersusAllTrainer>
* <xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>
* <xref:Microsoft.ML.Vision.ImageClassificationTrainer>

### <a name="multiclass-classification-inputs-and-outputs"></a>다중 클래스 분류 입력 및 출력

입력 레이블 열 데이터는 [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식이어야 합니다.
기능 열은 고정된 크기의 <xref:System.Single> 벡터여야 합니다.

이 트레이너는 다음을 출력합니다.

| 출력 이름 | 형식 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> 벡터 | 모든 클래스의 점수. 값이 높을수록 연결된 클래스에 해당할 가능성이 높습니다. i번째 요소의 값이 가장 크다면 예측된 레이블 인덱스는 i가 됩니다. i는 0부터 시작하는 인덱스입니다. |
| `PredictedLabel` | [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식 | 예측된 레이블의 인덱스입니다. 값이 i라면 실제 레이블은 키 값 입력 레이블 형식에서 i번째 범주입니다. |

## <a name="regression"></a>재발

관련 기능 집합에서 레이블 값을 예측하는 데 사용되는 [감독된 기계 학습](glossary.md#supervised-machine-learning) 작업입니다. 레이블은 실제 값일 수 있고, 분류 작업과 같이 한정된 값 집합에 속하지 않습니다. 회귀 알고리즘 모델은 기능 값이 달라질 때 레이블이 변경되는 방식을 결정하기 위한 관련 기능에 대한 레이블의 종속성입니다. 회귀 알고리즘의 입력은 알려진 값의 레이블이 포함된 예제 집합입니다. 회귀 알고리즘의 출력은 새 입력 기능 집합의 레이블 값을 예측하는 데 사용할 수 있는 함수입니다. 회귀 시나리오의 예는 다음과 같습니다.

* 침실 수, 위치 또는 규모와 같은 주택 특성을 기반으로 주택 가격 예측.
* 과거 데이터 및 현재 시장 추세를 기반으로 미래 재고 가격 예측.
* 광고 예산을 기반으로 제품 판매 예측.

### <a name="regression-trainers"></a>회귀 트레이너

다음 알고리즘을 사용하여 회귀 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>
* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>
* <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>
* <xref:Microsoft.ML.Trainers.OlsTrainer>
* <xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>

### <a name="regression-inputs-and-outputs"></a>회귀 입력 및 출력

입력 레이블 열 데이터는 <xref:System.Single>이어야 합니다.

이 작업에 대한 트레이너는 다음을 출력합니다.

| 출력 이름 | 형식 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 모델에서 예측된 원시 점수 |

## <a name="clustering"></a>

데이터 인스턴스를 비슷한 특성을 포함하는 클러스터로 그룹화하는 데 사용되는 [감독되지 않는 기계 학습](glossary.md#unsupervised-machine-learning) 작업입니다. 클러스터링을 사용하여 검색 또는 단순 관찰을 통해 논리적으로 파생될 수 없는 데이터 세트의 관계를 식별할 수도 있습니다. 클러스터링 알고리즘의 입력 및 출력은 선택한 방법에 따라 다릅니다. 분산, 중심, 연결 또는 밀도 기반 방법을 사용할 수 있습니다. 현재 ML.NET은 K-평균 클러스터링을 사용하는 중심 기반 방법을 지원합니다. 클러스터링 시나리오의 예는 다음과 같습니다.

* 호텔 선택의 습관과 특성을 기반으로 호텔 투숙객 세그먼트 이해.
* 대상 광고 캠페인을 구축하는 데 도움이 되는 고객 세그먼트 및 인구 통계 식별.
* 제조 메트릭을 기반으로 인벤토리 범주화.

### <a name="clustering-trainer"></a>클러스터 트레이너

다음 알고리즘을 사용하여 클러스터링 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.KMeansTrainer>

### <a name="clustering-inputs-and-outputs"></a>클러스터 입력 및 출력

입력 기능 데이터는 <xref:System.Single>이어야 합니다. 레이블이 필요하지 않습니다.

이 트레이너는 다음을 출력합니다.

| 출력 이름 | 형식 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> 벡터 | 모든 클러스터의 중심에서 특정 데이터 지점까지의 거리 |
| `PredictedLabel` | [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식 | 모델에서 예측한 가장 가까운 클러스터의 인덱스 |

## <a name="anomaly-detection"></a>변칙 검색

이 작업은 PCA(보안 주체 구성 요소 분석)를 사용하여 변칙 검색 모델을 만듭니다. PCA 기반 변칙 검색은 유효한 트랜잭션과 같은, 한 클래스의 학습 데이터를 얻기는 쉽지만 대상 변칙의 충분한 샘플을 얻기는 어려운 시나리오에서 모델을 빌드하는 데 도움이 됩니다.

기계 학습의 검증된 기술인 PCA는 데이터의 내부 구조를 나타내고 데이터 차이를 설명하기 때문에 탐색적 데이터 분석에서 자주 사용됩니다. PCA는 여러 변수를 포함하는 데이터를 분석하여 작동합니다. 변수 간의 상관 관계를 찾고 결과에서 차이점을 가장 잘 캡처하는 값의 조합을 확인합니다. 이러한 조합된 기능 값이 보안 주체 구성 요소라는 보다 간결한 기능 공간을 만드는 데 사용됩니다.

변칙 검색은 다음과 같은 기계 학습의 여러 중요한 작업을 포함합니다.

* 잠재적으로 사기성이 있는 트랜잭션 식별.
* 네트워크 침입이 발생했음을 나타내는 학습 패턴.
* 비정상적인 환자 클러스터 찾기.
* 시스템에 입력된 값 확인.

변칙은 정의상 거의 발생하지 않으므로 모델링에 사용할 데이터의 대표 샘플을 수집하기 어려울 수 있습니다. 이 범주에 포함된 알고리즘은 특히 불균형 데이터 세트를 사용하여 모델을 빌드하고 학습시키는 핵심 과제를 해결하도록 설계되었습니다.

### <a name="anomaly-detection-trainer"></a>변칙 검색 트레이너

다음 알고리즘을 사용하여 변칙 검색 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>

### <a name="anomaly-detection-inputs-and-outputs"></a>변칙 검색 입력 및 출력

입력 기능은 고정 크기의 <xref:System.Single> 벡터여야 합니다.

이 트레이너는 다음을 출력합니다.

| 출력 이름 | 형식 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 변칙 검색 모델에서 계산된 음수가 아니며 바인딩되지 않은 점수 |
| `PredictedLabel` | <xref:System.Boolean> | 입력이 변칙(PredictedLabel=true)인지 아닌지(PredictedLabel=false) 여부를 나타내는 true/false 값입니다. |

## <a name="ranking"></a>순위 지정

순위 지정 작업은 레이블이 지정된 예제 세트에서 순위매기기를 구성합니다. 이 예제 세트는 지정된 기준에 따라 점수가 매겨질 수 있는 인스턴스 그룹으로 이루어져 있습니다. 순위 레이블은 각 인스턴스마다 {0, 1, 2, 3, 4}입니다.  순위매기기는 각 인스턴스마다 점수를 알 수 없는 새 인스턴스 그룹의 순위를 지정하도록 학습됩니다. ML.NET 순위 지정 학습자는 [기계 학습 순위 지정](https://en.wikipedia.org/wiki/Learning_to_rank)을 기반으로 합니다.

### <a name="ranking-training-algorithms"></a>순위 지정 학습 알고리즘

다음 알고리즘을 사용하여 순위 지정 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>
* <xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>

### <a name="ranking-input-and-outputs"></a>순위 지정 입력 및 출력

입력 레이블 데이터 형식은 [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식 또는 <xref:System.Single>이어야 합니다. 레이블의 값은 관련성을 결정하며 값이 클수록 관련성이 높습니다. 레이블인 [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식이면 키 인덱스가 관련성 값이며 가장 작은 인덱스가 가장 관련도가 적습니다. 레이블이 <xref:System.Single>이면 값이 클수록 관련도가 높습니다.

기능 데이터는 고정 크기의 <xref:System.Single> 벡터여야 하며 입력 행 그룹 열은 [키](xref:Microsoft.ML.Data.KeyDataViewType) 형식이어야 합니다.

이 트레이너는 다음을 출력합니다.

| 출력 이름 | 형식 | 설명|
| -- | -- | -- |
| `Score` | <xref:System.Single> | 모델이 예측을 판단하기 위해 계산한 바인딩되지 않은 점수 |

## <a name="recommendation"></a>권장

권장 사항 작업을 통해 권장 제품 또는 서비스 목록을 생성할 수 있습니다. ML.NET은 카탈로그에 기록 제품 등급 데이터가 있는 경우 [공동 작업 필터링](https://en.wikipedia.org/wiki/Collaborative_filtering) 알고리즘인 [MF(행렬 인수)](https://en.wikipedia.org/wiki/Matrix_factorization_%28recommender_systems%29)를 권장 사항에 사용합니다. 예를 들어 사용자에 대한 기록 영화 등급 데이터가 있고, 사용자가 다음에 시청할 가능성이 큰 다른 영화를 추천하려고 합니다.

### <a name="recommendation-training-algorithms"></a>권장 사항 학습 알고리즘

다음 알고리즘을 사용하여 권장 사항 모델을 학습할 수 있습니다.

* <xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>

## <a name="forecasting"></a>예측

예측 작업은 과거 시계열 데이터를 사용하여 향후 동작에 대한 예측을 수행합니다. 예측에 적용되는 시나리오에는 날씨 예측, 계절별 매출 예측, 예측 유지 관리 등이 있습니다.

### <a name="forecasting-trainers"></a>트레이너 예측

다음 알고리즘을 사용하여 예측 모델을 학습할 수 있습니다.

<xref:Microsoft.ML.TimeSeriesCatalog.ForecastBySsa*>
