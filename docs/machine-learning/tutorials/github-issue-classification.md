---
title: '자습서: 지원 문제 분류 - 다중 클래스 분류'
description: 다중 클래스 분류 시나리오에서 ML.NET을 사용하여 GitHub 문제를 분류하여 지정된 영역에 할당하는 방법을 알아봅니다.
ms.date: 01/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: d5d397cfa6475574b6d6cc6d8cbb48b51c2d0af1
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2020
ms.locfileid: "77092527"
---
# <a name="tutorial-categorize-support-issues-using-multiclass-classification-with-mlnet"></a>자습서: ML .NET에서 다중 클래스 분류를 사용하여 지원 문제 분류

이 샘플 자습서에서는 ML.NET을 사용하여 Visual Studio에서 C#을 사용하는 .NET Core 콘솔 애플리케이션을 통해 GitHub 문제를 분류하고 영역 레이블을 예측하는 모델을 학습하는 방법에 대해 설명합니다.

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 살펴봅니다.
> [!div class="checklist"]
>
> * 데이터 준비
> * 데이터 변환
> * 모델 학습
> * 모델 평가
> * 학습된 모델을 통해 예측
> * 로드된 모델을 통해 배포 및 예측

[dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) 리포지토리에서 이 자습서의 소스 코드를 찾을 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* “.NET Core 플랫폼 간 개발” 워크로드가 설치된 [Visual Studio 2017 버전 15.6 이상](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017)

* [Github 이슈 탭으로 구분된 파일(issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv)을 발행합니다.
* [Github 이슈 테스트 탭으로 구분된 파일(issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv)을 발행합니다.

## <a name="create-a-console-application"></a>콘솔 애플리케이션 만들기

### <a name="create-a-project"></a>프로젝트 만들기

1. Visual Studio 2017을 엽니다. 메뉴 모음에서 **파일** > **새로 만들기** > **프로젝트**를 선택합니다. **새 프로젝트** 대화 상자에서 **Visual C#** 노드와 **.NET Core** 노드를 차례로 선택합니다. 그런 다음 **콘솔 앱(.NET Core)** 프로젝트 템플릿을 선택합니다. **이름** 텍스트 상자에 "GitHubIssueClassification"을 입력하고 **확인** 단추를 선택합니다.

2. 프로젝트에서 *Data* 디렉터리를 만들어 데이터 집합 파일을 저장합니다.

    **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 폴더**를 선택합니다. “Data”를 입력하고 Enter 키를 누릅니다.

3. 프로젝트에서 *Models* 디렉터리를 만들어 모델을 저장합니다.

    **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 폴더**를 선택합니다. "Models"를 입력하고 Enter 키를 누릅니다.

4. **Microsoft.ML NuGet 패키지**를 설치합니다.

    솔루션 탐색기에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다. "nuget.org"를 패키지 소스로 선택하고, [찾아보기] 탭을 선택하고, **Microsoft.ML**을 검색하고 **설치** 단추를 선택합니다. **변경 내용 미리 보기** 대화 상자에서 **확인** 단추를 선택한 다음, 나열된 패키지의 사용 조건에 동의하는 경우 **라이선스 승인** 대화 상자에서 **동의함** 단추를 선택합니다.

### <a name="prepare-your-data"></a>데이터 준비

1. [issues-train.csv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) 및 [issues-test.tsv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) 데이터 세트를 다운로드하여 이전에 만든 *Data* 폴더에 저장합니다. 첫 번째 데이터 세트는 기계 학습 모델을 교육하고 두 번째 데이터 세트는 모델이 얼마나 정확한지 평가하는 데 사용할 수 있습니다.

2. 솔루션 탐색기에서 각 \*.tsv 파일을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **고급** 아래에서 **출력 디렉터리에 복사** 값을 **변경된 내용만 복사**로 변경합니다.

### <a name="create-classes-and-define-paths"></a>클래스 만들기 및 경로 정의

*Program.cs* 파일 맨 위에 다음 추가 `using` 문을 추가합니다.

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddUsings)]

최근에 다운로드한 파일의 경로와 `MLContext`,`DataView` 및 `PredictionEngine`의 글로벌 변수가 포함될 세 개의 글로벌 필드를 만듭니다.

* `_trainDataPath`에는 모델을 학습시키는 데 사용되는 데이터 세트의 경로가 포함됩니다.
* `_testDataPath`에는 모델을 평가하는 데 사용되는 데이터 세트의 경로가 포함됩니다.
* `_modelPath`에는 학습된 모델이 저장되는 경로가 포함됩니다.
* `_mlContext`는 처리 컨텍스트를 제공하는 <xref:Microsoft.ML.MLContext>입니다.
* `_trainingDataView`는 학습 데이터 세트를 처리하는 데 사용되는 <xref:Microsoft.ML.IDataView>입니다.
* `_predEngine`은 단일 예측에 사용되는 <xref:Microsoft.ML.PredictionEngine%602>입니다.

`Main` 메서드 바로 위의 줄에 다음 코드를 추가하여 해당 경로와 다른 변수를 지정합니다.

[!code-csharp[DeclareGlobalVariables](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DeclareGlobalVariables)]

입력 데이터 및 예측에 대한 일부 클래스를 만듭니다. 새 클래스를 프로젝트에 추가합니다.

1. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **추가** > **새 항목**을 선택합니다.

1. **새 항목 추가** 대화 상자에서 **클래스**를 선택하고 **이름** 필드를 *GitHubIssueData.cs*로 변경합니다. 그런 다음, **추가** 단추를 선택합니다.

    *GitHubIssueData.cs* 파일이 코드 편집기에서 열립니다. 다음 `using` 문을 *GitHubIssueData.cs*의 맨 위에 추가합니다.

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#AddUsings)]

기존 클래스 정의를 제거하고 두 개의 클래스 `GitHubIssue` 및 `IssuePrediction`이 있는 다음 코드를 *GitHubIssueData.cs* 파일에 추가합니다.

[!code-csharp[DeclareGlobalVariables](~/samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#DeclareTypes)]

`label`은 예측할 열입니다. 식별된 `Features`는 레이블 예측을 위해 모델에 제공하는 입력입니다.

[LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute)를 사용하여 데이터 세트에서 원본 열의 인덱스를 지정합니다.

`GitHubIssue`는 다음 입력 데이터 세트 클래스이며 다음 <xref:System.String> 필드를 포함합니다.

* 첫 번째 열 `ID`(GitHub 문제 ID)
* 두 번째 열 `Area`(학습 예측)
* 세 번째 열 `Title`(GitHub 문제 제목)은 `Area` 예측에 사용되는 첫 번째 `feature`입니다.
* 네 번째 열 `Description`은 `Area` 예측에 사용되는 두 번째 `feature`입니다.

`IssuePrediction`은 모델이 학습된 후 예측에 사용되는 클래스입니다. 여기에는 단일 `string`(`Area`) 및 `PredictedLabel` `ColumnName` 특성이 포함됩니다.  `PredictedLabel`은 예측 및 평가 중에 사용됩니다. 평가를 위해 학습 데이터가 있는 입력, 예측 값 및 모델이 사용됩니다.

모든 ML.NET 작업은 [MLContext 클래스](xref:Microsoft.ML.MLContext)에서 시작됩니다. `mlContext`를 초기화하면 모델 생성 워크플로 개체 간에 공유할 수 있는 새 ML.NET 환경이 생성됩니다. 개념적으로 `Entity Framework`의 `DBContext`와 유사합니다.

### <a name="initialize-variables-in-main"></a>Main에서 변수 초기화

여러 학습에서 반복 가능한/결정적 결과를 얻기 위해 임의의 시드(`seed: 0`)가 있는 `MLContext`의 새 인스턴스로 `_mlContext` 글로벌 변수를 초기화합니다.  `Main` 메서드에서 `Console.WriteLine("Hello World!")` 줄을 다음 코드로 바꿉니다.

[!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a>데이터 로드

ML.NET은 숫자 또는 텍스트 테이블 형식 데이터를 설명하는 효율적인 방법으로 [IDataView 클래스](xref:Microsoft.ML.IDataView)를 유연하게 사용합니다. `IDataView`는 텍스트 파일을 또는 실시간으로 로드할 수 있습니다(예: SQL 데이터베이스 또는 로그 파일).

`_trainingDataView` 글로벌 변수를 파이프라인에 사용하기 위해 초기화 및 로드하려면 `mlContext` 초기화 후에 다음 코드를 추가합니다.

[!code-csharp[LoadTrainData](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTrainData)]

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29)은 데이터 스키마를 정의하고 파일에서 읽습니다. 데이터 경로 변수를 가져와서 `IDataView`를 반환합니다.

`Main` 메서드에 아래 코드를 다음 코드 줄로 추가합니다.

[!code-csharp[CallProcessData](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallProcessData)]

`ProcessData` 메서드는 다음 작업을 실행합니다.

* 데이터를 추출하고 변환합니다.
* 처리 파이프라인을 반환합니다.

다음 코드를 사용하여 `Main` 메서드 바로 뒤에 `ProcessData` 메서드를 만듭니다.

```csharp
public static IEstimator<ITransformer> ProcessData()
{

}
```

## <a name="extract-features-and-transform-the-data"></a>기능 추출 및 데이터 변환

`GitHubIssue`에 대한 영역 GitHub 레이블을 예측하기 위해 [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 메서드를 사용하여 `Area` 열을 숫자 키 형식 `Label` 열로 변환하고(분류 알고리즘에서 허용하는 형식) 새 데이터 세트 열로 추가합니다.

[!code-csharp[MapValueToKey](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#MapValueToKey)]

다음으로, 텍스트(`Title` 및 `Description`) 열을 `TitleFeaturized` 및 `DescriptionFeaturized` 각각에서 숫자 벡터로 변환하는 `mlContext.Transforms.Text.FeaturizeText`를 호출합니다. 다음 코드를 사용하여 두 열에 대한 기능화(featurization)를 파이프라인에 추가합니다.

[!code-csharp[FeaturizeText](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#FeaturizeText)]

데이터 준비의 마지막 단계에서는 [Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) 메서드를 사용하여 모든 기능 열을 **Features** 열에 결합합니다. 기본적으로, 학습 알고리즘은 **Features** 열의 기능만 처리합니다. 다음 코드를 사용하여 이 변환을 파이프라인에 추가합니다.

[!code-csharp[Concatenate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Concatenate)]

 다음으로, <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A>를 추가하여 DataView를 캐시합니다. 따라서 해당 캐시를 사용하여 데이터를 여러 번 반복하면 다음 코드를 사용하는 것처럼 성능이 향상될 수 있습니다.

[!code-csharp[AppendCache](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AppendCache)]

> [!WARNING]
> 작거나 중간 규모의 데이터 세트에서는 AppendCacheCheckpoint를 사용하여 학습 시간을 단축하세요. 규모가 매우 큰 데이터 세트를 다룰 때는 사용하지 마세요(AppendCacheCheckpoint()를 제거).

`ProcessData` 메서드의 끝에 파이프라인을 반환합니다.

[!code-csharp[ReturnPipeline](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnPipeline)]

이 단계는 전처리/기능화를 처리합니다. ML.NET에서 사용 가능한 추가 구성 요소를 사용하면 모델에서 더 나은 결과를 얻을 수 있습니다.

## <a name="build-and-train-the-model"></a>모델 빌드 및 학습

`BuildAndTrainModel` 메서드에 다음 호출을 `Main` 메서드의 다음 코드 줄로 추가합니다.

[!code-csharp[CallBuildAndTrainModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallBuildAndTrainModel)]

`BuildAndTrainModel` 메서드는 다음 작업을 실행합니다.

* 학습 알고리즘 클래스를 만듭니다.
* 모델을 학습시킵니다.
* 학습 데이터를 기반으로 영역을 예측합니다.
* 모델을 반환합니다.

다음 코드를 사용하여 `Main` 메서드 바로 뒤에 `BuildAndTrainModel` 메서드를 만듭니다.

```csharp
public static IEstimator<ITransformer> BuildAndTrainModel(IDataView trainingDataView, IEstimator<ITransformer> pipeline)
{

}
```

### <a name="about-the-classification-task"></a>분류 작업 정보

분류는 데이터를 사용하여 데이터 항목 또는 행의 범주, 형식 또는 클래스를 **확인**하는 기계 학습 작업으로, 보통은 다음 형식 중 하나입니다.

* 이진: A 또는 B.
* 다중 클래스: 단일 모델을 사용하여 예측할 수 있는 여러 범주.

이러한 유형의 문제에는 다중 클래스 분류 학습 알고리즘을 사용합니다. 문제 범주 예측이 단 2개(이진)가 아닌 여러 범주(다중 클래스) 중 하나일 수 있기 때문입니다.

`BuildAndTrainModel()`의 첫 번째 코드 줄로 다음을 추가하여 데이터 변환 정의에 기계 학습 알고리즘을 추가합니다.

[!code-csharp[AddTrainer](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTrainer)]

[SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer)는 다중 클래스 분류 학습 알고리즘입니다. 이것은 `pipeline`에 추가되고 기록 데이터에서 학습할 기능화된 `Title`, `Description`(`Features`) 및 `Label` 입력 매개 변수를 수락합니다.

### <a name="train-the-model"></a>모델 학습

모델을 `splitTrainSet` 데이터에 맞추고 `BuildAndTrainModel()` 메서드에서 다음 줄의 코드로 다음 항목을 추가하여 학습된 모델을 반환합니다.

[!code-csharp[TrainModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#TrainModel)]

`Fit()` 메서드는 데이터 세트를 변환하고 학습을 적용하여 모델을 학습합니다.

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602)은 데이터의 단일 인스턴스를 전달한 다음, 이 단일 데이터 인스턴스에 대한 예측을 수행할 수 있는 편리한 API입니다. 이 항목을 `BuildAndTrainModel()` 메서드에서 다음 줄로 추가합니다.

[!code-csharp[CreatePredictionEngine1](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine1)]

### <a name="predict-with-the-trained-model"></a>학습된 모델을 통해 예측

`GitHubIssue`의 인스턴스를 만들어 GitHub 문제를 추가하여 `Predict` 메서드에서 학습된 모델의 예측을 테스트합니다.

[!code-csharp[CreateTestIssue1](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateTestIssue1)]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 함수를 사용하여 단일 데이터 행에 대한 예측을 수행합니다.

[!code-csharp[Predict](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Predict)]

### <a name="using-the-model-prediction-results"></a>모델 사용: 예측 결과

결과를 공유하고 이에 따라 작업을 수행하기 위해 `GitHubIssue` 및 해당 `Area` 레이블 예측을 표시합니다.  다음 <xref:System.Console.WriteLine?displayProperty=nameWithType> 코드를 사용하여 결과 표시를 만듭니다.

[!code-csharp[OutputPrediction](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#OutputPrediction)]

### <a name="return-the-model-trained-to-use-for-evaluation"></a>평가에 사용하기 위해 학습된 모델 반환

`BuildAndTrainModel` 메서드의 끝에 모델을 반환합니다.

[!code-csharp[ReturnModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnModel)]

## <a name="evaluate-the-model"></a>모델 평가

이제 모델을 만들고 학습시켰으므로 품질 보증 및 유효성 검사를 위해 다른 데이터 세트를 사용하여 평가해야 합니다. `Evaluate` 메서드에서는 `BuildAndTrainModel`에서 만들어진 모델을 전달하여 평가합니다. 다음 코드와 같이 `BuildAndTrainModel` 바로 뒤에 `Evaluate` 메서드를 만듭니다.

```csharp
public static void Evaluate(DataViewSchema trainingDataViewSchema)
{

}
```

`Evaluate` 메서드는 다음 작업을 실행합니다.

* 테스트 데이터 세트를 로드합니다.
* 다중 클래스 평가자를 만듭니다.
* 모델을 평가하고 메트릭을 만듭니다.
* 메트릭을 표시합니다.

다음 코드를 사용하여 `BuildAndTrainModel` 메서드 호출 바로 아래에 `Main` 메서드의 새 메서드 호출을 추가합니다.

[!code-csharp[CallEvaluate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallEvaluate)]

앞의 학습 데이터 세트에서처럼 `Evaluate` 메서드에 다음 코드를 추가하여 테스트 데이트 세트를 로드합니다.

[!code-csharp[LoadTestDataset](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTestDataset)]

[Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) 메서드는 지정된 데이터 세트를 사용하여 모델에 대한 품질 메트릭을 계산합니다. 다중 클래스 분류 평가자가 계산한 전체 메트릭을 포함하는 <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> 개체를 반환합니다.
모델의 품질을 확인하기 위해 메트릭을 표시하려면 먼저 해당 메트릭을 가져와야 합니다.
기계 학습 `_trainedModel` 글로벌 변수([ITransformer](xref:Microsoft.ML.ITransformer))의 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 메서드를 사용하여 기능을 입력하고 예측을 반환합니다. `Evaluate` 메서드에 아래 코드를 다음 줄로 추가합니다.

[!code-csharp[Evaluate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Evaluate)]

다중 클래스 분류에 대한 다음 메트릭이 계산됩니다.

* 마이크로 정확도 - 모든 샘플 클래스 쌍은 정확도 메트릭에 동일하게 기여합니다.  마이크로 정확도를 가능한 한 1에 가깝게 합니다.

* 매크로 정확도 - 모든 클래스 정확도 메트릭에 동일하게 기여합니다. 소수 클래스는 큰 클래스와 같은 가중치를 부여받습니다. 매크로 정확도를 가능한 한 1에 가깝게 합니다.

* 로그 손실 - [로그 손실](../resources/glossary.md#log-loss)을 참조하세요. 로그 손실을 가능한 한 0에 가깝게 합니다.

* 로그 손실 감소 - [-inf, 1.00]의 범위입니다. 여기서 1.00은 완벽한 예측이고 0은 평균 예측을 나타냅니다. 로그 손실 감소를 가능한 한 1에 가깝게 합니다.

### <a name="displaying-the-metrics-for-model-validation"></a>모델 유효성 검사를 위해 메트릭 표시

다음 코드를 사용하여 메트릭을 표시하고, 결과를 공유한 다음, 작업을 수행합니다.

[!code-csharp[DisplayMetrics](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayMetrics)]

### <a name="save-the-model-to-a-file"></a>모델을 파일에 저장

모델이 만족스러운 경우 나중에 또는 다른 애플리케이션에서 예측을 수행하기 위해 모델을 파일에 저장합니다. `Evaluate` 메서드에 다음 코드를 추가합니다.

[!code-csharp[SnippetCallSaveModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SnippetCallSaveModel)]

`Evaluate` 메서드 아래 `SaveModelAsFile` 메서드를 만듭니다.

```csharp
private static void SaveModelAsFile(MLContext mlContext,DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

`SaveModelAsFile` 메서드에 다음 코드를 추가합니다. 이 코드에서는 [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save*) 메서드를 사용하여 학습된 모델을 직렬화하고 zip 파일로 저장합니다.

[!code-csharp[SnippetSaveModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SnippetSaveModel)]

## <a name="deploy-and-predict-with-a-model"></a>모델을 통해 배포 및 예측

다음 코드를 사용하여 `Evaluate` 메서드 호출 바로 아래에 `Main` 메서드의 새 메서드 호출을 추가합니다.

[!code-csharp[CallPredictIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallPredictIssue)]

다음 코드를 사용하여 `Evaluate` 메서드 바로 뒤에 (또한 `SaveModelAsFile` 메서드 바로 앞에) `PredictIssue` 메서드를 만듭니다.

```csharp
private static void PredictIssue()
{

}
```

`PredictIssue` 메서드는 다음 작업을 실행합니다.

* 저장된 모델 로드
* 테스트 데이터의 단일 문제를 만듭니다.
* 테스트 데이터를 기반으로 영역을 예측합니다.
* 보고를 위해 테스트 데이터 및 예측을 결합합니다.
* 예측 결과를 표시합니다.

다음 코드를 `PredictIssue` 메서드에 추가하여 저장된 모델을 애플리케이션에 로드합니다.

[!code-csharp[SnippetLoadModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#SnippetLoadModel)]

`GitHubIssue`의 인스턴스를 만들어 GitHub 문제를 추가하여 `Predict` 메서드에서 학습된 모델의 예측을 테스트합니다.

[!code-csharp[AddTestIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTestIssue)]

이전에 수행한 것처럼 다음 코드로 `PredictionEngine` 인스턴스를 만듭니다.

[!code-csharp[CreatePredictionEngine](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602)은 데이터의 단일 인스턴스에 대한 예측을 수행할 수 있는 편리한 API입니다. [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)은 스레드로부터 안전하지 않습니다. 단일 스레드 또는 프로토타입 환경에서 사용할 수 있습니다. 프로덕션 환경에서 성능 및 스레드 보안을 개선하려면 `PredictionEnginePool` 서비스를 사용합니다. 이 서비스는 애플리케이션 전체에서 사용할 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 개체의 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)을 만듭니다. [ASP.NET Core Web API에서 `PredictionEnginePool`을 사용하는 방법](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)에 대한 이 가이드를 참조하세요.

> [!NOTE]
> `PredictionEnginePool` 서비스 확장은 현재 미리 보기 상태입니다.

`PredictionEngine`을 사용하여 예측을 위해 다음 코드를 `PredictIssue` 메서드에 추가하여 영역 GitHub 레이블을 예측합니다.

[!code-csharp[PredictIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#PredictIssue)]

### <a name="using-the-loaded-model-for-prediction"></a>예측에 로드된 모델 사용

문제를 분류하고 이에 따라 조치를 취하기 위해 `Area`를 표시합니다. 다음 <xref:System.Console.WriteLine?displayProperty=nameWithType> 코드를 사용하여 결과 표시를 만듭니다.

[!code-csharp[DisplayResults](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayResults)]

## <a name="results"></a>결과

다음과 같은 결과가 나타나야 합니다. 파이프라인이 처리할 때 메시지를 표시합니다. 경고 또는 메시지 처리를 확인할 수 있습니다. 이해하기 쉽도록 이러한 메시지는 다음 결과에서 제거되었습니다.

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.738
*       MacroAccuracy:    0.668
*       LogLoss:          .919
*       LogLossReduction: .643
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

지금까지 이제 GitHub 문제의 영역 레이블을 분류하고 예측하기 위한 기계 학습 모델을 성공적으로 빌드했습니다. [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification) 리포지토리에서 이 자습서의 소스 코드를 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

본 자습서에서는 다음 작업에 관한 방법을 학습했습니다.
> [!div class="checklist"]
>
> * 데이터 준비
> * 데이터 변환
> * 모델 학습
> * 모델 평가
> * 학습된 모델을 통해 예측
> * 로드된 모델을 통해 배포 및 예측

다음 자습서로 이동하여 자세히 알아보기
> [!div class="nextstepaction"]
> [택시 요금 예측기](predict-prices.md)
