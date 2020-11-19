---
layout: post
title:  "Exchange Tips"
date:   2020-11-20 00:00:00 +0300
categories: tips
---

## Smtp Errors

### 504 5.7.4 Unrecognized authentication type

Начиная с версии Exchange 2010, по умолчанию включен только механизм аутентификации NTLM. Два способа решения этой ошибки:

+ a) настроить приложение, отправляющее письма, на использование схемы аутентификации NTLM  
+ b) включить другие схемы аутентификации на сервере Excange:  
  + server configuration - hub transport  
  + выбрать сервер
  + выбрать свойства нужного receive connector  
  + вкладка authentication  
  + установить галочку Basic Authentication  
  + снять галочку Offer Basic only after TLS
  + Перезапустить службы Excange.
