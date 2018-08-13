## 0 단계 : 리소스 설정

### 단계 0A : 람다 함수 및 데이터 저장소 설정

이 워크샵에서 만들 AWS Step Functions 상태 머신은 각 단계에 대한 로직을 구현하는 다수의 람다 함수를 조정합니다. 이러한 람다 함수 중 일부는 Amazon S3 버킷 또는 Amazon DynamoDB 테이블과 같은 AWS 리소스 및 데이터 저장소의 존재에 의존합니다.

이 섹션에서는 AWS 람다 함수와 필요한 모든 리소스를 프로비저닝하기 위해 클라우드 템플릿을 사용합니다.

이 단계에서 어떤 리소스가 설정되어 있는지 이해하려면 아래 다이어그램을 참조하십시오. 이 워크샵에서는 처리 작업을 수행하는 람다 함수를 조정하기 위해 단계 함수 상태 머신 (중간에 회색으로 표시됨)을 빌드합니다.
<br/>

<img src="images/0-resource-setup.png" width="65%" height="65%">

<br/>
<br/>

Region| Code | Launch
------|------|-------
미국 동부 (오하이오)| <span style="font-family:'Courier';">us-east-2</span> | [![Launch Step 0A in us-east-2](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-us-east-2.amazonaws.com/sfn-image-workshop-us-east-2/cloudformation/step0-sam.yaml)
미국 동부 (버지니아) | <span style="font-family:'Courier';">us-east-1</span> | [![Launch Step 0A in us-east-1](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3.amazonaws.com/sfn-image-workshop-us-east-1/cloudformation/step0-sam.yaml)
미국 서부 (오레곤) | <span style="font-family:'Courier';">us-west-2</span> | [![Launch Step 0A in us-west-2](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-us-west-2.amazonaws.com/sfn-image-workshop-us-west-2/cloudformation/step0-sam.yaml)
EU (아일랜드) | <span style="font-family:'Courier';">eu-west-1</span> | [![Launch Step 0A in eu-west-1](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-eu-west-1.amazonaws.com/sfn-image-workshop-eu-west-1/cloudformation/step0-sam.yaml)
도쿄 | <span style="font-family:'Courier';">ap-northeast-1</span> | [![Launch Step 0A in ap-northeast-1](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-ap-northeast-1.amazonaws.com/sfn-image-workshop-ap-northeast-1/cloudformation/step0-sam.yaml)
서울 | <span style="font-family:'Courier';">ap-northeast-2</span> | [![Launch Step 0A in ap-northeast-2](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-ap-northeast-2.amazonaws.com/sfn-image-workshop-ap-northeast-2/cloudformation/step0-sam.yaml)
Sydney | <span style="font-family:'Courier';">ap-southeast-2</span> | [![Launch Step 0A in ap-southeast-2](images/cfn-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=sfn-workshop-setup&templateURL=https://s3-ap-southeast-2.amazonaws.com/sfn-image-workshop-ap-southeast-2/cloudformation/step0-sam.yaml)


<details>
<summary><strong> CloudFormation 실행 지침 (자세한 내용은 펼치기) </strong></summary><p>
 
1. 위의 **Launch Stack** 버튼을 클릭하십시오.

1. Select Template 페이지에서 **Next**를 클릭하십시오.

1. 세부 정보 지정 페이지에서 모든 기본값을 그대로 두고 **다음**을 클릭합니다.

1. 옵션 페이지에서 기본값을 모두 그대로 두고 **다음**을 클릭하십시오.

1. 검토 페이지에서 확인란을 선택하여 CloudFormation이 IAM 리소스를 만들고 **변경 집합 만들기**를 클릭하는지 확인합니다.
	![IAM 스크린 샷 승인](./images/0a-cfn-create-change-set.png)

	이 템플리트는 람다가 처리해야하는 자원에 대한 적절한 사용 권한을 부여하는 많은 IAM 역할을 작성합니다. 그 외에도 템플릿은 [AWS Serverless Application Model]을 활용하는 [AWS Serverless Transform](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/transform-aws-serverless.html) (https://github.com/awslabs/serverless-application-model) - SAM - 서버가없는 구성 요소의 템플리트 작성을 단순화합니다.

1. 변경 사항 설정이 완료되면 컴퓨팅 변경 사항을 완료하고 **실행**을 클릭하십시오.
	![Change Change Set Screenshot](./images/0a-cfn-execute-change-set.png)

1. `sfn-workshop-setup` 스택이 `CREATE_COMPLETE`의 상태에 도달 할 때까지 기다리십시오. (새로 생성 된 스택을 보려면 refresh 버튼을 클릭해야 할 수도 있습니다).
</details>

	
### 다음 단계
이제 [1 단계](step-1.md)로 이동할 준비가 되었습니다!