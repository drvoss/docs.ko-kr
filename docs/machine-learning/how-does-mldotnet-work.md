---
title: ML.NET은 무엇이며 어떻게 작동하나요?
description: ML.NET은 온라인 또는 오프라인 시나리오에서 .NET 애플리케이션에 기계 학습을 추가할 수 있는 기능을 제공합니다. 이 기능을 사용하면 ML.NET을 사용하기 위해 네트워크에 연결할 필요 없이 애플리케이션에 사용 가능한 데이터를 사용하여 자동 예측할 수 있습니다. 이 문서에서는 ML.NET에서 기계 학습의 기본 사항을 설명합니다.
ms.date: 11/5/2019
ms.topic: overview
ms.custom: mvc
ms.openlocfilehash: bc157b22201c66bceecf78aaa36b9c653fe6a131
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794570"
---
# <a name="what-is-mlnet-and-how-does-it-work"></a>ML.NET은 무엇이며 어떻게 작동하나요?

ML.NET은 온라인 또는 오프라인 시나리오에서 .NET 애플리케이션에 기계 학습을 추가할 수 있는 기능을 제공합니다. 이 기능을 사용하여 애플리케이션에 사용 가능한 데이터를 사용하여 자동 예측할 수 있습니다. 기계 학습 애플리케이션은 명시적으로 프로그래밍하지 않아도 데이터의 패턴을 사용하여 예측합니다.

ML.NET의 핵심은 기계 학습 **모델**입니다. 모델은 입력 데이터를 예측으로 변환하는 데 필요한 단계를 지정합니다. ML.NET를 사용하면 알고리즘을 지정하여 사용자 지정 모델을 학습하거나 미리 학습된 TensorFlow 및 ONNX 모델을 가져올 수 있습니다.

모델이 있는 경우, 애플리케이션에 이를 추가하여 예측할 수 있습니다.

ML.NET은 .NET Core를 사용하여 Windows, Linux 및 macOS에서 실행되거나 .NET Framework를 사용하여 Windows를 실행됩니다. 64비트는 모든 플랫폼에서 지원됩니다. 32비트는 TensorFlow, LightGBM 및 ONNX 관련 기능을 제외하고 Windows에서 지원됩니다.

ML.NET을 사용하여 수행할 수 있는 예측 유형은 다음과 같습니다.

|||
|-|-|
|분류/범주화|고객 피드백을 긍정과 부정 범주로 자동 구분|
|재발/연속 값 예측|크기 및 위치에 따라 주택 가격 예측|
|변칙 검색|사기성 은행 거래 검색 |
|권장 사항|이전 구매 내역에 기반하여 온라인 구매자가 구매하려는 제품 제안|
|시계열/순차 데이터|날씨/제품 판매 예측|
|이미지 분류|의료 이미지에서 병리학 분류|

## <a name="hello-mlnet-world"></a>Hello ML.NET World

다음 코드 조각의 코드는 가장 단순한 ML.NET 애플리케이션을 보여줍니다. 이 예제에서는 집 크기 및 가격 데이터를 사용하여 주택 가격을 예측하도록 선형 회귀 모델을 구성합니다. 

 ```csharp
    using System;
    using Microsoft.ML;
    using Microsoft.ML.Data;

    class Program
    {
        public class HouseData
        {
            public float Size { get; set; }
            public float Price { get; set; }
        }

        public class Prediction
        {
            [ColumnName("Score")]
            public float Price { get; set; }
        }

        static void Main(string[] args)
        {
            MLContext mlContext = new MLContext();

            // 1. Import or create training data
            HouseData[] houseData = {
                new HouseData() { Size = 1.1F, Price = 1.2F },
                new HouseData() { Size = 1.9F, Price = 2.3F },
                new HouseData() { Size = 2.8F, Price = 3.0F },
                new HouseData() { Size = 3.4F, Price = 3.7F } };
            IDataView trainingData = mlContext.Data.LoadFromEnumerable(houseData);

            // 2. Specify data preparation and model training pipeline
            var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
                .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));

            // 3. Train model
            var model = pipeline.Fit(trainingData);

            // 4. Make a prediction
            var size = new HouseData() { Size = 2.5F };
            var price = mlContext.Model.CreatePredictionEngine<HouseData, Prediction>(model).Predict(size);

            Console.WriteLine($"Predicted price for size: {size.Size*1000} sq ft= {price.Price*100:C}k");

            // Predicted price for size: 2500 sq ft= $261.98k
        }
    }
```

## <a name="code-workflow"></a>코드 워크플로

다음 다이어그램은 반복적인 모델 개발 프로세스뿐만 아니라 애플리케이션 코드 구조를 나타냅니다.

- 학습 데이터를 수집하여 **IDataView** 개체로 로드
- 기능을 추출하고 기계 학습 알고리즘을 적용할 작업 파이프라인 지정
- 파이프라인에서 **Fit()** 를 호출하여 모델 학습
- 모델을 평가하고 반복하여 개선
- 애플리케이션에서 사용할 수 있도록 모델을 이진 형식으로 저장
- 모델을 **ITransformer** 개체로 다시 로드
- **CreatePredictionEngine.Predict()** 를 호출하여 예측

![데이터 생성, 파이프라인 개발, 모델 학습, 모델 평가 및 모델 사용에 대한 구성 요소를 포함한 ML.NET 애플리케이션 개발 흐름](./media/mldotnet-annotated-workflow.png)

이러한 개념을 좀 더 깊이 살펴 보겠습니다.

## <a name="machine-learning-model"></a>기계 학습 모델

ML.NET 모델은 예측된 출력에 도달하기 위해 입력 데이터에서 수행할 변형이 포함된 개체입니다.

### <a name="basic"></a>Basic

가장 기본적인 모델은 위의 주택 가격 예에서와 같이 하나의 지속적인 수량이 다른 것과 비례하는 2차원 선형 회귀입니다.

![편차와 가중치 매개 변수가 포함된 선형 회귀 모델](./media/linear-regression-model.svg)

모델은 단순히 $Price = b + Size * w$입니다. 매개 변수 $b$ 및 $w$는 쌍 세트(크기, 가격)에 줄을 맞춰 추정됩니다. 모델의 매개 변수를 찾는 데 사용되는 데이터를 **학습 데이터**라고 합니다. 이 기계 학습 모델의 입력을 **특성(feature)**이라고 합니다. 이 예제에서는 $Size$가 유일한 특성입니다. 기계 학습 모델을 학습하는 데 사용하는 실측 자료(ground-truth) 값은 **레이블**이라고 합니다. 여기에서는 학습 데이터 세트의 $Price$ 값이 레이블입니다.

### <a name="more-complex"></a>더 복잡한

더 복잡한 모델은 거래 텍스트 설명을 사용하여 금융 거래를 범주로 분류합니다.

각 거래 설명은 중복되는 단어와 문자를 제거하고 단어와 문자 조합을 계산하여 일련의 기능으로 세분화됩니다. 기능 집합은 학습 데이터에서 범주 집합에 기반하여 선형 모델을 학습하는 데 사용됩니다. 새로운 설명이 학습 집합의 설명과 유사할수록 동일한 범주에 할당될 가능성이 커집니다.

![텍스트 분류 모델](./media/text-classification-model.svg)

주택 가격 책정 모델 및 텍스트 분류 모델 둘 다 **선형** 모델입니다. 데이터의 성격과 해결하려는 문제에 따라, **결정 트리**, **일반화된 부가** 모델 등을 사용할 수도 있습니다. [작업](./resources/tasks.md)에서 모델에 대해 더 자세히 알아볼 수 있습니다.

## <a name="data-preparation"></a>데이터 준비

대부분의 경우에서 사용자가 가지고 있는 사용 가능한 데이터는 기계 학습 모델을 학습하기 위해 직접 사용하기에는 적합하지 않습니다. 원시 데이터는 준비하거나 사전 처리되어야 모델의 매개 변수를 찾기 위해 사용할 수 있습니다. 데이터를 문자열 값에서 숫자 표현으로 변환해야 합니다. 사용자의 입력 데이터에서 중복되는 정보가 있을 수 있습니다. 입력 데이터의 크기를 축소 또는 확장해야 할 수 있습니다. 데이터를 정규화 또는 크기 조정해야 할 수 있습니다.

[ML.NET 자습서](./tutorials/index.md)는 특정 기계 학습 작업에 사용되는 텍스트, 이미지, 숫자 및 시계열 데이터에 대한 다른 데이터 처리 파이프라인에 대해 알려줍니다.

[데이터 준비 방법](./how-to-guides/prepare-data-ml-net.md)은 데이터 준비를 더 일반적으로 적용하는 방법을 보여줍니다.

리소스 섹션에서 모든 [사용 가능한 변환](./resources/transforms.md)의 부록을 확인할 수 있습니다.

## <a name="model-evaluation"></a>모델 평가

모델을 학습한 후에는 미래의 예측을 얼마나 잘 할지 어떻게 알까요? ML.NET를 사용하여 새로운 테스트 데이터를 기준으로 모델을 평가할 수 있습니다.

기계 학습 작업의 각 유형에는 테스트 데이터 세트를 기준으로 모델의 정확성 및 정밀도를 평가하는 데 사용하는 메트릭이 있습니다.

주택 가격 예의 경우 **회귀** 작업을 사용했습니다. 모델을 평가하려면 원래 샘플에 다음 코드를 추가합니다.

```csharp
        HouseData[] testHouseData =
        {
            new HouseData() { Size = 1.1F, Price = 0.98F },
            new HouseData() { Size = 1.9F, Price = 2.1F },
            new HouseData() { Size = 2.8F, Price = 2.9F },
            new HouseData() { Size = 3.4F, Price = 3.6F }
        };

        var testHouseDataView = mlContext.Data.LoadFromEnumerable(testHouseData);
        var testPriceDataView = model.Transform(testHouseDataView);

        var metrics = mlContext.Regression.Evaluate(testPriceDataView, labelColumnName: "Price");

        Console.WriteLine($"R^2: {metrics.RSquared:0.##}");
        Console.WriteLine($"RMS error: {metrics.RootMeanSquaredError:0.##}");

        // R^2: 0.96
        // RMS error: 0.19
```

평가 메트릭은 오류가 낮은 수준이며 예측한 출력과 테스트 출력 간 상관 관계가 높다는 것을 알려줍니다. 이 과정은 아주 쉽습니다! 실제 사례에서는 훌륭한 모델 메트릭을 얻는 데 더 많은 튜닝이 필요합니다.

## <a name="mlnet-architecture"></a>ML.NET 아키텍처

이 섹션에서는 ML.NET의 아키텍처 패턴을 살펴봅니다. 숙련된 .NET 개발자라면 이러한 패턴의 일부는 익숙하고 일부는 덜 익숙해집니다. 자세히 살펴보는 동안 긴장을 놓지 마세요!

ML.NET 애플리케이션은 <xref:Microsoft.ML.MLContext> 개체로 시작합니다. 이 싱글톤 개체는 **카탈로그**를 포함합니다. 카탈로그는 데이터 로드 및 저장, 변환, 트레이너, 모델 작동 구성 요소를 위한 팩터리입니다. 각 카탈로그 개체마다 다른 유형의 구성 요소를 만드는 메서드가 있습니다.

|||||
|-|-|-|-|
|데이터 로드 및 저장||<xref:Microsoft.ML.DataOperationsCatalog>||
|데이터 준비||<xref:Microsoft.ML.TransformsCatalog>||
|학습 알고리즘|이진 분류|<xref:Microsoft.ML.BinaryClassificationCatalog>||
||다중 클래스 분류|<xref:Microsoft.ML.MulticlassClassificationCatalog>||
||변칙 검색|<xref:Microsoft.ML.AnomalyDetectionCatalog>||
||Clustering|<xref:Microsoft.ML.ClusteringCatalog>||
||예측|<xref:Microsoft.ML.ForecastingCatalog>||
||순위 지정|<xref:Microsoft.ML.RankingCatalog>||
||재발|<xref:Microsoft.ML.RegressionCatalog>||
||권장|<xref:Microsoft.ML.RecommendationCatalog>|`Microsoft.ML.Recommender` NuGet 패키지 추가|
||TimeSeries|<xref:Microsoft.ML.TimeSeriesCatalog>|`Microsoft.ML.TimeSeries` NuGet 패키지 추가|
|모델 사용 ||<xref:Microsoft.ML.ModelOperationsCatalog>||

위의 각 범주에서 만들기 메서드로 이동할 수 있습니다. 카탈로그는 Visual Studio를 사용하여 IntelliSense를 통해 나타납니다.

   ![회귀 트레이너용 Intellisense](./media/catalog-intellisense.png)

### <a name="build-the-pipeline"></a>파이프라인 빌드

각 카탈로그 내에는 확장 메서드의 집합이 있습니다. 확장 메서드를 사용하여 학습 파이프라인을 만드는 방법을 살펴 보겠습니다.

```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
        .Append(mlContext.Regression.Trainers.Sdca(labelColumnName: "Price", maximumNumberOfIterations: 100));
```

코드 조각에서 `Concatenate` 및 `Sdca`는 카탈로그에 있는 두 메서드입니다. 이러한 메서드는 파이프라인에 연결된 [IEstimator](xref:Microsoft.ML.IEstimator%601) 개체를 만듭니다.

이 시점에서는 개체만 만들어집니다. 어떠한 실행도 발생하지 않습니다.

### <a name="train-the-model"></a>모델 학습

파이프라인에서 개체가 만들어지면 모델을 학습하는 데 데이터를 사용할 수 있습니다.

```csharp
    var model = pipeline.Fit(trainingData);
```

`Fit()` 호출 시 입력 학습 데이터를 사용하여 모델의 매개 변수를 예상합니다. 이를 모델 학습이라고 합니다. 단, 위의 선형 회귀 모델에서는 두 가지 모델 매개 변수, 즉 **편차**와 **가중치**가 있었습니다. `Fit()` 호출 후 매개 변수의 값이 알려집니다. 대부분 모델에는 이것보다 더 많은 매개 변수가 있습니다.

[모델 학습 방법](./how-to-guides/train-machine-learning-model-ml-net.md)에서 모델 학습에 대해 자세히 알아볼 수 있습니다.

결과 모델 개체는 <xref:Microsoft.ML.ITransformer> 인터페이스를 구현합니다. 즉, 모델은 입력 데이터를 예측으로 변환합니다.

```csharp
   IDataView predictions = model.Transform(inputData);
```

### <a name="use-the-model"></a>모델 사용

입력 데이터를 대량의 예측으로 변환하거나 한 번에 하나의 입력을 변환할 수 있습니다. 주택 가격 예에서는 두 가지(모델 평가 목적으로는 대량, 새로운 예측을 하기 위해서는 한 번에 하나씩)를 수행했습니다. 단일 예측하기를 살펴 보겠습니다.

```csharp
    var size = new HouseData() { Size = 2.5F };
    var predEngine = mlContext.CreatePredictionEngine<HouseData, Prediction>(model);
    var price = predEngine.Predict(size);
```

`CreatePredictionEngine()` 메서드는 입력 클래스 및 출력 클래스를 사용합니다. 필드 이름 및/또는 코드 특성은 모델 학습 및 예측 중 사용된 데이터 열의 이름을 결정합니다. 방법 섹션에서 [단일 예측하는 방법](./how-to-guides/single-predict-model-ml-net.md)에 대해 읽을 수 있습니다.

### <a name="data-models-and-schema"></a>데이터 모델 및 스키마

ML.NET 기계 학습 파이프라인의 핵심에는 [DataView](xref:Microsoft.ML.IDataView) 개체가 있습니다.

파이프라인의 각 변환에는 입력 스키마(변환 시 입력에서 보길 원하는 데이터 이름, 유형 및 크기)와 출력 스키마(변환 후 해당 변환이 생성하는 데이터 이름, 유형 및 크기)가 있습니다. 다음 문서에서는 [IDataView 인터페이스 및 관련 형식 시스템](https://xadupre.github.io/machinelearningext/mlnetdocs/idataviewtypesystem.html)에 대한 자세한 설명을 제공합니다.

파이프라인에서 하나의 변환으로 인한 출력 스키마가 다음 변환의 입력 스키마와 일치하지 않는 경우 ML.NET은 예외를 throw합니다.

데이터 뷰 개체에는 열과 행이 있습니다. 각 열에는 이름과 유형 및 길이가 있습니다. 예: 주택 가격 예의 입력 열에는 **크기**와 **가격**이 있습니다. 두 가지 유형이며, 벡터 수량이라기 보다는 스칼라 수량입니다.

   ![주택 가격 예측 데이터가 있는 ML.NET 데이터 뷰 예제](./media/ml-net-dataview.png)

모든 ML.NET 알고리즘은 벡터인 입력 열을 찾습니다. 기본적으로 이 벡터 열은 **특징(feature)**이라고 합니다. 이것이 바로 주택 가격 예제에서 **Features**이라고 하는 새 열로 **크기** 열을 연관시킨 이유입니다.

 ```csharp
    var pipeline = mlContext.Transforms.Concatenate("Features", new[] { "Size" })
 ```

또한 모든 알고리즘은 예측을 수행한 후 새 열을 만들기도 합니다. 이러한 새 열의 고정된 이름은 기계 학습 알고리즘의 유형에 따라 결정됩니다. 회귀 작업의 경우 새 열 중 하나가 **점수**라고 합니다. 이것이 바로 이 이름으로 가격 데이터의 이름을 지정한 이유입니다.

```csharp
    public class Prediction
    {
        [ColumnName("Score")]
        public float Price { get; set; }
    }
```

[Machine Learning 작업](resources/tasks.md) 가이드에서 다른 기계 학습 작업의 출력 열에 대해 더 자세히 알아볼 수 있습니다.

DataView의 중요한 속성은 이들이 **느리게** 평가된다는 점입니다. 데이터 뷰는 모델 학습 및 평가, 데이터 예측 중에만 로드되고 작동됩니다. ML.NET 애플리케이션을 쓰고 테스트하는 동안에는 Visual Studio 디버거를 사용하여 [미리 보기](xref:Microsoft.ML.DebuggerExtensions.Preview*) 메서드를 호출하여 어떠한 데이터 뷰 개체도 살펴볼 수 있습니다.

```csharp
    var debug = testPriceDataView.Preview();
```

디버거에서 `debug` 변수를 보고 내용을 살펴볼 수 있습니다. 성능이 크게 저하되므로 프로덕션 코드에서는 미리 보기 메서드를 사용하지 마세요.

### <a name="model-deployment"></a>모델 배포

실제 애플리케이션에서 모델 학습과 평가 코드는 예측과 구분됩니다. 사실,이 두 가지 활동은 별도의 팀에서 수행하는 경우가 많습니다. 모델 개발 팀은 예측 애플리케이션에서 사용하기 위해 이 모델을 저장할 수 있습니다.

```csharp
   mlContext.Model.Save(model, trainingData.Schema,"model.zip");
```

## <a name="next-steps"></a>다음 단계

* [자습서](./tutorials/index.md)에서 보다 현실적인 데이터 세트로 다른 기계 학습 작업을 사용하여 애플리케이션을 빌드하는 방법에 대해 알아봅니다.

* [방법 가이드](./how-to-guides/index.md)에서 더 자세히 특정 항목에 대해 알아봅니다.

* 만일 관심이 아주 많다면 [API 참조 문서](https://docs.microsoft.com/dotnet/api/?view=ml-dotnet)를 바로 참조할 수 있습니다.
