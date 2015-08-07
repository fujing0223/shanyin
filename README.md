*** Settings ***
Library           Requests Library
Library           Collections

*** Test Cases ***
用户请求（有custId）
    [Tags]    post
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://authtest.wecash.net    auth=${auth}
    Set Test Variable    ${data_req}    {"name":"张测试","idCard":"11010119160701265X","phone":"18500000099","creditSesame":"999","jobName":"1111","source":${11111},"custId":${29001}}
    ${headers}=    Create Dictionary    Content-Type    text/json    charset    UTF-8
    ${resp}=    Post request    req    /authBaseInfo    data=${data_req}    headers=${headers}
    ${data_resp}=    To Json    ${resp.content}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}

用户请求（无custId）
    [Tags]    post
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    request    https://authtest.wecash.net    auth=${auth}
    Set Test Variable    ${data_req}    {"name":"张测试","idCard":"11010119160701265X","phone":"18500000099","creditSesame":"999","jobName":"","source":${11111}}
    ${headers}=    Create Dictionary    Content-Type    text/json    charset    UTF-8
    ${resp}=    Post request    request    /authBaseInfo    data=${data_req}    headers=${headers}
    ${data_resp}=    To Json    ${resp.content}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}

京东授权跳转
    [Tags]    get
    Create Session    login    http://authtest.wecash.net:8611
    ${resp}=    Get request    login    /credit_jd.html?custId=29001&source=11111&returnUrl=http://www.wecash.net&channel=ios
    Should Be Equal As Strings    ${resp.status_code}    200

运营商授权跳转
    [Tags]    get
    Create Session    login    http://authtest.wecash.net:8611
    ${resp}=    Get request    login    /credit_operator.html?custId=29001&source=11111&returnUrl=http://www.wecash.net&channel=ios
    Should Be Equal As Strings    ${resp.status_code}    200

淘宝授权跳转
    Create session    login    http://authtest.wecash.net:8611
    ${resp}    get    login    /credit_taobao.html?custId=${29001}&source= ${11111}&returnUrl=http://www.wecash.net&channel=ios
    Should Be equal As strings    ${resp.status_code}    200

通用授权查询
    [Tags]    post
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://authtest.wecash.net    auth=${auth}
    Set Test Variable    ${data_req}    {"source":${11111},"custId":${29001}}
    ${resp}=    Post request    req    /query    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}

京东查询
    [Tags]    post
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://datacenter-test.wecash.net    auth=${auth}
    Set Test Variable    ${data_req}    {"jd_account":"xiaoshan1505","source":${11111},"custId":${29001}}
    ${resp}=    Post request    req    /protype-jingdong/getInfo    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}

打分查询
    [Tags]    post
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://datacenter-test.wecash.net    auth=${auth}
    Set Test Variable    ${data_req}    {"source":${11111},"custId":${29001}}
    ${resp}=    Post request    req    /protype-score/getInfo    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}

淘宝查询
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://datacenter-test.wecash.net    auth=${auth}
    ${data_req}    Set Variable    {"tb_account":"15801038625","source":${11111},"custId":${29001}}
    ${resp}=    Post request    req    /protype-taobao/getInfo    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    log    ${data_resp}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}

运营商查询-北京移动
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://datacenter-test.wecash.net    auth=${auth}
    ${data_req}    Set Variable    {"phone":"15801038625",'source':'${11111}','custId':'${29001}'}
    ${resp}=    Post request    req    /protype-operator/getInfo    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    log    ${data_resp}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}

运营商授权-北京联通
    ${auth}=    Create List    wecashtest    Qvu2XTlSDSIySZEP7x36
    Create Session    req    https://datacenter-test.wecash.net    auth=${auth}
    ${data_req}    Set Variable    {"phone":"18511431900",'source':'${11111}','custId':'${29001}'}
    ${resp}=    Post request    req    /protype-operator/getInfo    data=${data_req}
    ${data_resp}=    To Json    ${resp.content}
    log    ${data_resp}
    Should Be Equal As Strings    ${resp.status_code}    200
    Should Be Equal As Integers    ${data_resp['msg']}    ${0}
    Should Not Be Empty    ${data_resp['result']}
