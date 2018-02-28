---
layout: post
title:  "[The model layer] 1-2. Model field reference"
date:   2018-02-28 11:30:00 +0900
categories: django
---

구글 번역기의 도움을 (많이) 받아 장고 공식 문서를 번역하였습니다.

# Model field reference

이 문서는 장고가 제공하는 필드 옵션과 필드 유형을 포함하여 Field의 모든 API 레퍼런스를 포함한다.

> 기술적으로, 이들 모델은 django.db.models.fields에 정의되어 지만 편의상 django.db.models로 가져온다.  
> 표준 규칙은 `from django.db import models`를 사용하고 models.<Foo>Field를 참조하는 것이다. 

