## 5 단계 : S3 이벤트에서 실행 시작

마지막으로 상태 시스템이 가동됩니다! 실행을 자동화 할 차례입니다.
이미지가 착륙 S3 버킷에 업로드 될 때 상태 시스템이 트리거되기를 원합니다.

여기에는 두 가지 가능성이 있습니다.

1. CloudWatch Events를 사용하여 S3 버킷의 활동을 모니터링하고 단계 기능을 실행 대상으로 선택합니다

1. 다음과 같은 람다 함수를 사용하십시오 :
	- 상태 시스템을 호출합니다.
	- S3 이벤트에 의해 트리거됩니다.

첫 번째 단계는 CloudTrail을 활성화해야하기 때문에 몇 가지 추가 구성이 필요합니다. 간결성을 위해 옵션 2를 사용할 것입니다.

### 5A 단계 : StartExecution을 지시합니다. Lambda 함수는 단계 기능이 트리거 될 상태 머신을 결정합니다.

1. [AWS Step Functions 관리 콘솔](http://console.aws.amazon.com/states/home)로 이동하십시오. AWS Region 선택이 지금까지 작업 한 AWS Region 선택과 일치하는지 확인하십시오.

1. 대시 보드에서 상태 시스템을 찾습니다. 이전 단계에 대한 지시 사항을 따른 경우 * ImageProcessing *

1. 페이지의 오른쪽 상단 모서리에있는**State Machine Arn**:

	<img src="images / 5a-state-machine-arn-newer.png" width="90%">

1. [AWS Lambda 관리 콘솔](https://console.aws.amazon.com/lambda/home)로 이동하여 이름이 'StartExecution'으로 끝나는 람다 함수를 찾으십시오. 이것이 상태 시스템을 트리거하는 것입니다. 해당 이름의 링크를 클릭하여 선택하십시오.

1. 실행되는 특정 AWS Step Function 상태 머신은 구성 가능한 환경 변수로 람다 함수에 전달됩니다.**환경 변수**섹션으로 스크롤하십시오. 그것을 확장하고 자리 표시 자 값`FILL_WITH_YOUR_VALUE`을 상태 머신의 ARN으로 변경하십시오.

	<img src="images / 5a-enviroment-variables.png" width="90%">

1. 페이지 상단으로 스크롤 한 다음**저장**을 클릭하십시오.

### 단계 5B : StartExecution 람다 함수를 트리거하도록 S3 이벤트 설정

람다 함수는 이제 우리가 실행시키고 자하는 상태 머신을 알고 있습니다. 이제는 람다 함수를 트리거하는 이벤트를 설정해야합니다. 새 객체가 랜딩 버켓에 업로드 될 때마다 자동으로 실행되기를 원하기 때문에 S3 이벤트에 의해 람다 함수가 트리거되어야합니다.

1. S3 관리 콘솔로 이동하여 랜딩 버킷을 선택하십시오.

	```
	sfn-workshop-setup-photorepos3bucket-xxxxxxxxxxxxx
	```

1. 폴더를 만들고 전화 * 수신 *

	<img src="images / 5b-s3-incoming-folder.png" width="70%">

1.**속성**탭을 클릭하십시오.

1.**이벤트**를 클릭하십시오.

	<img src="images / 5b-s3-events.png" width="80%">

1.**알림 추가**를 클릭하십시오.

1. 다음 매개 변수를 입력하십시오.
	-**Name**: ExecuteStateMachine
	-**Events**: ObjectCreate(All)
	-**Prefix**: Incoming/
	-**Send to**: Lambda Function
	-**Lambda**: sfn-workshop-setup-StartExecution

	>**Note:**the**Prefix**parameter is critical: this limits the event trigger to only trigger processing workflows when an image file lands in the "Incoming/" prefix. Because the thumbnail generation process uploads the thumbnails to the same S3 bucket, without limiting the prefix, the thumbnail upload will trigger another workflow and causes an infinite loop.
	
	<img src="images/5b-s3-event-configuration.png" width="60%">
	
1.**저장**을 클릭하십시오.

### 5C 단계 : S3에 사진을 업로드하여 이벤트 트리거 테스트

이제 이벤트를 테스트 할 준비가되었습니다! S3 버킷 내의 이미지를 "Incoming"폴더에 업로드하고 Step Functions 콘솔에서 실행을 확인하십시오!

1. S3 관리 콘솔에서 랜딩 버켓의 'Incoming /'프리픽스로 이동하여**Upload**를 클릭하십시오. 지원되는 형식 (JPEG 또는 PNG)의 이미지를 선택하십시오.**다음을 클릭하십시오**


1.**다음**및**업로드**를 클릭하십시오.

1. 상태 시스템이 트리거되고 성공적으로 실행되는지 확인하십시오. DynamoDB에 저장된 축소판 및 데이터를 확인합니다.

	<img src="images / 5c-state-machine-execution.png" width="90%">


### 다음 단계
이제 [Step 6] (step-6.md)로 이동할 준비가되었습니다!

