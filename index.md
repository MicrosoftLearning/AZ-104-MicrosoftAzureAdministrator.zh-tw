---
title: 線上託管說明
permalink: index.html
layout: home
ms.openlocfilehash: 1dde36744b9541205d719973757171e13ec37223
ms.sourcegitcommit: 8a0ced6338608682366fb357c69321ba1aee4ab8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2021
ms.locfileid: "145198075"
---
# <a name="content-directory"></a>內容目錄

您可以[在此下載](https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/archive/master.zip)必要的實驗室檔案

下方列出可連至各實驗室活動的超連結。

## <a name="labs"></a>實驗室

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| 模組 | 實驗室 |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}


