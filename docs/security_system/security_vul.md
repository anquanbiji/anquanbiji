# 常见安全漏洞列表 



## 2017版 OWASP top 10 

常见安全漏洞列表,比较知名的为owasp整理的[owasp top 10](https://owasp.org/www-project-top-ten/ "最常见的前10个安全漏洞"),对应翻译的中文文档[OWASP 前10安全漏洞中文版](https://wiki.owasp.org/images/d/dc/OWASP_Top_10_2017_%E4%B8%AD%E6%96%87%E7%89%88v1.3.pdf),[owasp top 10 github issues](https://github.com/OWASP/Top10/issues)  
owasp top 10 是在收集了大量社区反馈意见和企业组织数据基础上，汇编整理出来的，比较有权威性，并且在随着安全漏洞的发展，不断的更新, 当前最新版是2017版  

《OWASP Top 10》的首要目的是教导开发人员、设计人员、架构师、管理人员和企业组织，让他们认识到最严重
Web应用程序安全弱点所产生的后果。Top 10提供了防止这些高风险问题发生的基本方法，并为获得这些方法的提供了
指引。


###  注入漏洞

将不受信任的数据作为命令或查询的一部分发送到**解析器**时，会产生诸如SQL注入、NoSQL注入、OS注入和LDAP注入缺陷。攻击者的恶意数据可以诱使解析器在没有适当授权的情况下执行非预期命令或访问数据。 

注释:解释型语言在执行时解析，因此如果在输入数据的位置，输入成指令，如SQL指令，解释器就会执行指令  

**应用描述**  
几乎任何数据源都能成为注入载体,包括环境变量、所有类型的用户参数、外部和内部web服务。当攻击者可以向解释器发送恶意数据时，注入漏洞产生 

**安全弱点**   
注入漏洞十分普遍，尤其是在遗留代码中。注入漏洞通常能在SQL、LDAP、XPath或是NoSQL查询语句、OS命令、XML解析器、SMTP包头、表达式语句及ORM查询语句中找到 

注入漏洞很容易通过代码审计发现。扫描器和模糊测试工具可以帮助攻击者找到这些漏洞。 

**影响** 

注入能导致数据丢失、破坏或泄露给无授权方，缺乏可审计性或是拒绝服务。注入有时甚至能导致主机被完全接管。

您的应用和数据需要受到保护，以避免对业务造成影响 

**如何防止?**  
防止注入漏洞需要将数据与命令语句、查询语句分隔开来 

- 最佳选择是使用安全的API，完全避免使用解释器，或提供参数化界面的接口，或迁移到ORM框架 
- 使用正确的输入验证方法有助于防止注入攻击，但者不是一个完整的防御，因为许多应用程序在输入中需要特殊字符，例如文本区域或移动应用程序的API  
- 对于任何剩余的动态查询，可以使用该解释器的特定转义语法转义特殊字符。OWASP的Java Encoder和类似的库提供了这样的转义例程  
- 在查询中使用LIMIT和其他SQL控件，以防止在SQL注入时大量地泄露记录  



### 失效的身份认证

通过错误使用应用程序的身份认证和会话管理功能，攻击者能够破译密码、密钥或会话令牌，或者利用其它开发缺陷来暂时性或永久性冒充其他用户的身份。

**应用描述**  
攻击者可以获得数百万的有效用户名和密码组合，包括证书填充、默认的管理账户列表、自动的暴力破解和字典攻击工具，以及高级的GPU破解工具。会话管理攻击很容易被理解，尤其是没有过期的会话密钥

**安全弱点**  
大多数身份和访问管理系统的设计和实现，普遍存在身份认证失效问题。会话管理是身份验证和访问控制的基础，并且存在于所有有状态的应用程序中

攻击者可以使用指南手册来检测失效的身份验证，但通常会关注密码转储、字典攻击，或者在类似于钓鱼或社会工程攻击之后，发现失效的身份认证 

**影响**  
攻击者只需要访问几个账户，或者只需要一个管理员账户就可以破坏我们的系统。根据应用程序领域的不同，可能会导致社会欺诈、用户身份盗窃、泄露法律高度保护的敏感信息 

**如何防止?**

- 在可能的情况下，实现多因素身份验证，以防止自动、凭证填充、暴力破解和被盗凭据再利用攻击  
- 不要使用发送或部署默认的凭据，特别是管理员账户  
- 执行弱密码检查，例如测试新或变更的密码，以纠正常见的弱密码 
- 指定密码策略:密码长度、复杂度、修改策略等 
- 确认注册、登录功能， 提示信息模糊化，防范账户枚举攻击 
- 限制或逐渐延迟失败的登录尝试。记录所有失败信息并在凭据填充、暴力破解或其他攻击被检测时提醒管理员 
- 使用服务器端安全的内置会话管理器，在登录后生成高度复杂的新随机会话ID。会话ID不能在URL中，可以安全地存储和当登出、闲置、绝对超时后使其失效

### 敏感数据泄露

许多web应用程序和API都无法正确保护敏感数据，例如:财务数据、医疗数据和PII数据。攻击者可以通过窃取或修改未加密的数据来实施信用卡诈骗、身份盗窃或其他犯罪行为。未加密的敏感数据容易受到破坏，因此我们需要对敏感数据加密，这些数据包括:传输过程中的数据、存储的数据及浏览器的交互数据

**应用描述**  
攻击者不是直接攻击密码，而是在传输过程中或从客户端（例如：浏览器）窃取密钥、发起中间人攻击，或从服
务器端窃取明文数据。这通常需要手动攻击。

**安全弱点**  
在最近几年，这是最常见的、最具影响力的攻击。这个领域最常见的漏洞是**不对敏感信息进行加密**。在数据加密过程中，常见的问题是不安全的密钥生成和管理以及使用弱加密算法、弱协议和弱密码。特别是使用弱的哈希算法来保护密码。在服务器端，检测传输过程中的数据弱点很容易，但检测存储数据的弱点却非常困难。

**影响**  
这个领域的错误频繁影响那些本应该加密的数据。通常情况下，这些数据包括很多个人敏感信息（PII），例如：医疗记录、认证凭证、个人隐私、信用卡信息等。这些信息受到相关法律和条例保护，例如：欧盟《通用数据保护条例》（GDPR）和地方隐私保护法律。

**如何防止?**  

对一些需要加密的敏感数据，应该起码做到以下几点： 

- 对系统处理、存储或传输的数据分类，并根据分类进行访问控制。
- 熟悉与敏感数据保护相关的法律和条例，并根据每项法规要求保护敏感数据。
- 对于没必要存放的、重要的敏感数据，应当尽快清除，或者通过PCI DSS标记或拦截。未存储的数据不能被窃取。
- 确保存储的所有敏感数据被加密。
- 确保使用了最新的、强大的标准算法或密码、参数、协议和密匙，并且密钥管理到位。
- 确保传输过程中的数据被加密，如：使用TLS。确保数据加密被强制执行，如：使用HTTP严格安全传输协议（HSTS ）。
- 禁止缓存对包含敏感数据的响应。
- 确保使用密码专用算法存储密码，如：Argon2 、 scrypt 、bcrypt 或者PBKDF2 。将工作因素（延迟因素）设置在可接受范围。
- 单独验证每个安全配置项的有效性。

### XML外部实体(XXE)

许多较早的或配置错误的XML处理器评价了XML文件中的外部实体引用。攻击者可以利用外部实体窃取使用URI文件处理器的内部文件和共享文件、监听内部扫描端口、执行远程代码和实施拒绝服务攻击。

**应用描述**  

如果攻击者可以上传XML文档或者在XML文档中添加恶意内容，通过易受攻击的代码、依赖项或集成，他们就能够攻击含有缺陷的XML处理器。

**安全弱点**   
默认情况下，许多旧的XML处理器能够对外部实体、XML进程中被引用和评估的URI进行规范。SAST 工具可以通过检查依赖项和安全配置来发现XXE缺陷。DAST工具需要额外的手动步骤来检测和利用XXE缺陷。因为XXE漏洞测试在2017年并不常见，因此手动测试人员需要通过接受培训来了解如何进行XXE漏洞测试。

**影响**    
XXE缺陷可用于提取数据、执行远程服务器请求、扫描内部系统、执行拒绝服务攻击和其他攻击。业务影响取决于所有受影响的应用程序和数据保护需求。 

**如何防止?**  
开发人员培训是识别和减少XXE缺陷的关键，此外，防止XXE 缺陷还需要:  

- 尽可能使用简单的数据格式（如：JSON），避免对敏感数据进行序列化。
- 及时修复或更新应用程序或底层操作系统使用的所有XML处理器和库。同时，通过依赖项检测，将SOAP更新到1.2版本或更高版本。
- 参考《 OWASP Cheat Sheet ‘XXE Prevention‘ 》，在应用程序的所有XML解析器中禁用XML外部实体和DTD进程。
- 在服务器端实施积极的（“白名单”）输入验证、过滤和清理，以防止在XML文档、标题或节点中出现恶意数据。
- 验证XML或XSL文件上传功能是否使用XSD验证或其他类似验证方法来验证上传的XML文件。
- 尽管在许多集成环境中，手动代码审查是大型、复杂应用程序的最佳选择，但是SAST 工具可以检测源代码中的XXE漏洞。

如果无法实现这些控制，请考虑使用虚拟修复程序、API安全网关或Web应用程序防火墙（ WAF ）来检测、监控和防止XXE攻击。


### 失效的访问控制 

未对通过身份验证的用户实施恰当的访问控制。攻击者可以利用这些缺陷访问未经授权的功能或数据，例如:访问其他用户的账户、查看敏感文件、修改其他用户的数据、更改访问权限等 

**应用描述**  

对访问控制的利用是渗透测试人员的一项核心技能。SAST 工具和 DAST工具可以检测到访问控制的缺失，但不能验证其功能是否正常。访问控制可通过手动方式检测，或在某些特定框架下通过自动化检测访问控制缺失。

**安全弱点**   
由于缺乏自动化的检测和应用程序开发人员缺乏有效的功能测试，因而访问控制缺陷很常见。访问控制检测通常不适用于自动化的静态或动态测试。手动测试是检测访问控制缺失或失效的最佳方法，包括：HTTP方法（如：GET和PUT）、控制器、直接对象引用等。


**影响**    
技术影响是攻击者可以冒充用户、管理员或拥有特权的用户，或者创建、访问、更新或删除任何记录。业务影响取决于应用程序和数据的保护需求。


**如何防止?**  
访问控制只有在受信服务器端代码或没有服务器的 API 中有效，这样这样攻击者才无法修改访问控制检查或元数据。

- 除公有资源外，默认情况下拒绝访问。
- 使用一次性的访问控制机制，并在整个应用程序中不断重用它们，包括最小化CORS使用。
- 建立访问控制模型以强制执行所有权记录，而不是接受用户创建、读取、更新或删除的任何记录。
- 域访问控制对每个应用程序都是唯一的，但业务限制要求应由域模型强制执行。
- 禁用 Web服务器目录列表，并确保文件元数据（如：git）不存在于 Web的根目录中。
- 记录失败的访问控制，并在适当时向管理员告警
- 对API和控制器的访问进行速率限制，以最大限度地降低自动化攻击工具的危害。
- 当用户注销后，服务器上的JWT令牌应失效。 
开发人员和 QA人员应包括功能访问控制单元和集成测试人员。


### 安全配置错误

由于不安全的默认配置、不完整的临时配置、开源云存储、错误的HTTP标记头配置以及包含敏感信息的详细错误信息所造成的。因此，我们不仅需要对所有的操作系统、框架、库和应用程序进行安全配置，而且必须及时修补和升级它们

**应用描述**  
通常，攻击者能够通过未修复的漏洞、访问默认账户、不再使用的页面、未受保护的文件和目录等来取得对系统的未授权的访问。


**安全弱点**  
安全配置错误可以发生在一个应用程序堆栈的任何层面，包括网络服务、平台、Web服务器、应用服务器、数据库、框架、自定义代码和预安装的虚拟机、容器和存储。自动扫描器可用于检测错误的安全配置、默认帐户的使用或配置、不必要的服务、遗留选项等。


**影响**    

这些漏洞使攻击者能经常访问一些未授权的系统数据或功能。有时，这些漏洞导致系统的完全攻破。业务影响取决于您的应用程序和数据的保护需求。

**如何防止?**  
应实施安全的安装过程，包括： 
- 一个可以快速且易于部署在另一个锁定环境的可重复的加固过程。开发、质量保证和生产环境都应该进行相同配置，并且，在每个环境中使用不同的密码。这个过程应该是自动化的，以尽量减少安装一个新安全环境的耗费。
- 搭建最小化平台，该平台不包含任何不必要的功能、组件、文档和示例。移除或不安装不适用的功能和框架。
- 检查和修复安全配置项来适应最新的安全说明、更新和补丁，并将其作为更新管理过程的一部分。在检查过程中，应特别注意云存储权限（如：S3桶权限）。
- 一个能在组件和用户间提供有效的分离和安全性的分段应用程序架构，包括：分段、容器化和云安全组。
- 向客户端发送安全指令，如：安全标头。
- 在所有环境中能够进行正确安全配置和设置的自动化过程。


### 跨站脚本(XSS)

当应用程序的新网页中包含不受信任的、未经恰当验证或转义的数据时，或者使用可以创建HTML或Javascript的浏览器API更新现有的网页时，就会出现XSS缺陷。XSS让攻击者能够在受害者的浏览器中执行脚本，并劫持用户会话、破坏网站或将用户重定向到恶意站点 

**应用描述**  
自动化工具能够检测并利用所有的三种XSS形式，并且存在方便攻击者利用漏洞的框架。

**安全弱点**  
XSS是OWASP Top10中第二普遍的安全问题，存在于近三分之二的应用中。自动化工具能自动发现一些XSS问题，特别是在一些成熟的技术中，如：PHP、J2EE或JSP、ASP.NET。


**影响**    
XSS对于反射和DOM的影响是中等的，而对于存储的XSS，XSS的影响更为严重，譬如在受攻击者的浏览器上执行远程代码，例如：窃取凭证和会话或传递恶意软件等。


**如何防止?**  
防止XSS需要将不可信数据与动态的浏览器内容区分开。这可以通过如下步骤实现： 

- 使用设计上就会自动编码来解决XSS问题的框架，如：Ruby 3.0或 React JS。了解每个框架的XSS保护的局限性，并适当地处理未覆盖的用例。 
- 为了避免反射式或存储式的XSS漏洞，最好的办法是根据HTML输出的上下文（包括：主体、属性、JavaScript、CSS或URL）对所有不可信的HTTP请求数据进行恰当的转义 。更多关于数据转义技术的信息见：《OWASP Cheat Sheet ‘XSS Prevention’》
- 在客户端修改浏览器文档时，为了避免DOM XSS攻击，最好的选择是实施上下文敏感数据编码。如果这种情况不能避免，可以采用《OWASP Cheat Sheet ‘DOM based XSS Prevention ‘》描述的类似上下文敏感的转义技术应用于浏览器API。
- 使用内容安全策略（CSP）是对抗XSS的深度防御策略。如果不存在可以通过本地文件放置恶意代码的其他漏洞（例如：路径遍历覆盖和允许在网络中传输的易受攻击的库），则该策略是有效的。

### 不安全的反序列化

会导致远程代码执行。即使反序列化缺陷不会导致远程代码执行，攻击者也可以利用他们来执行攻击，包括:重放攻击、注入攻击和特权升级攻击 

**应用描述**  
对反序列化的利用是有点困难的。因为在不更改或调整底层可被利用代码的情况下，现成的反序列化漏洞很难被使用。



**安全弱点**  
这一问题包括在Top 10的行业调查中，而不是基于可量化的数据。有些工具可以被用于发现反序列化缺陷，但经常需要人工帮助来验证发现的问题。希望有关反序列化缺陷的普遍性数据将随着工具的开发而被更多的识别和解决。

**影响**    
反序列化缺陷的影响不能被低估。它们可能导致远程代码执行攻击，这是可能发生的最严重的攻击之一。业务影响取决于应用程序和数据的保护需求。


**如何防止?**  
唯一安全的架构模式是不接受来自不受信源的序列化对象，或使用只允许原始数据类型的序列化媒体。如果上述不可能的话，考虑使用下述方法： 

- 执行完整性检查，如：任何序列化对象的数字签名，以防止恶意对象创建或数据篡改。 
- 在创建对象之前强制执行严格的类型约束，因为代码通常被期望成一组可定义的类。绕过这种技术的方法已经被证明，所以完全依赖于它是不可取的。
- 如果可能，隔离运行那些在低特权环境中反序列化的代码。
- 记录反序列化的例外情况和失败信息，如：传入的类型不是预期的类型，或者反序列处理引发的例外情况。
- 限制或监视来自于容器或服务器传入和传出的反序列化网络连接。
- 监控反序列化，当用户持续进行反序列化时，对用户进行警告。

### 使用含有已知漏洞的组件 

组件(例如:库、框架和其他软件模块)拥有和应用程序相同的权限，如果应用程序中含有已知漏洞的组件被攻击者利用，可能会造成严重的数据丢失或服务器接管。同时，使用含有已知漏洞的组件应用程序和API可能会破坏应用程序防御、造成各种攻击并产生严重影响

**应用描述**  
对一些漏洞很容易找到其利用程序，但对其它的漏洞则需要定制开发。


**安全弱点**  
这种安全漏洞普遍存在。基于组件开发的模式使得多数开发团队根本不了解其应用或API中使用的组件，更谈不上及时更新这些组件了。如Retire.js之类的扫描器可以帮助发现此类漏洞，但这类漏洞是否可以被利用还需花费额外的时间去研究。

**影响**    
虽然对于一些已知的漏洞其影响很小，但目前很多严重的安全事件都是利用组件中的已知漏洞。根据你所要保护的资产，此类风险等级可能会很高。


**如何防止?**  
应该制定一个补丁管理流程：  
- 移除不使用的依赖、不需要的功能、组件、文件和文档。
- 利用如 versions、DependencyCheck 、retire.js等工具来持续的记录客户端和服务器端以及它们的依赖库的版本信息。持续监控如CVE 和 NVD等是否发布已使用组件的漏洞信息，可以使用软件分析工具来自动完成此功能。订阅关于使用组件安全漏洞的警告邮件。
- 仅从官方渠道安全的获取组件，并使用签名机制来降低组件被篡改或加入恶意漏洞的风险
- 监控那些不再维护或者不发布安全补丁的库和组件。如果不能打补丁，可以考虑部署虚拟补丁来监控、检测或保护。
每个组织都应该制定相应的计划，对整个软件生命周期进行监控、评审、升级或更改配置。


### 不足的日志记录和监控

不足的日志记录和监控，以及事件响应缺失或无效的集成，使攻击者能够进一步攻击系统、保持持续性或攻击更多系统，篡改、提取或销毁数据。大多数缺陷研究显示，缺陷被检测出的时间超过200天，且通常通过外部检测方检测，而不是通过内部流程或监控检测 

**应用描述**  
对不足的日志记录及监控的利用几乎是每一个重大安全事件的温床。攻击者依靠监控的不足和响应的不及时来达成他们的目标而不被知晓。

**安全弱点**  
根据行业调查的结果，此问题被列入了Top 10。判断你是否有足够监控的一个策略是在渗透测试后检查日志。 测试者的活动应被充分的记录下来，能够反映出他们造成了什么样的影响。

**影响**    
多数成功的攻击往往从漏洞探测开始。允许这种探测会将攻击成功的可能性提高到近100%据统计，在2016年确定一起数据泄露事件平均需要花191天时间，这么长时间里损害早已发生。


**如何防止?**  
根据应用程序存储或处理的数据的风险：
- 确保所有登录、访问控制失败、输入验证失败能够被记录到日志中去，并保留足够的用户上下文信息，以识别可疑或恶意帐户，并为后期取证预留足够时间。
- 确保日志以一种能被集中日志管理解决方案使用的形式生成
- 确保高额交易有完整性控制的审计信息，以防止篡改或删除，例如审计信息保存在只能进行记录增加的数据库表中。
- 建立有效的监控和告警机制，使可疑活动在可接受的时间内被发现和应对。
- 建立或采取一个应急响应机制和恢复计划，例如：NIST 800-61 rev 2或更新版本。

目前已有商业的和开源的应用程序防护框架（例如：OWASPAppSensor）、Web应用防火墙（例如 ：Modsecurity with theOWASP Core Rule Set）、带有自定义仪表盘和告警功能的日志关联软件。

## CWE 
CWE是Common Weakness Enumeration的缩写，官方网站[cwe.mitre.org](https://cwe.mitre.org/),网站主要功能是收集漏洞/问题列表，漏洞缓解措施，预防方案 。既包括软件漏洞也包括硬件(汽车、IoT、工控系统、可穿戴设备)漏洞  

[软件开发](https://cwe.mitre.org/data/definitions/699.html)   
[硬件设计](https://cwe.mitre.org/data/definitions/1194.html)  
[研究概念](https://cwe.mitre.org/data/definitions/1000.html)

推出了最严重的25个漏洞 [2021_cwe_top25](https://cwe.mitre.org/top25/archive/2021/2021_cwe_top25.html),不仅是web，还包括软件如释放后重用

有很多问题例子，并且根据语言进行划分，非常不错 

共计992个问题列表，暂且不完全列举 


## burp suite web security 

[教程列表](https://portswigger.net/web-security/learning-path)  适合安全专业人员学习，学习后做对应的题



## 解决方案 

### 研发人员下一步做什么  

**建立并使用可重复使用的安全流程和标准的安全控制**  
对任何人来说，创建一个安全的web应用程序都可能很困难。若您需要管理一个大型的应用程序系统群，那任务将十分艰巨  

为帮助企业和开发人员以节省成本的方式降低应用程序的安全风险，OWASP创建了相当多的免费和开源的资源。您可以使用这些资源来解决您企业组织的应用程序安全问题。以下内容是OWASP为帮助企业组织创建安全的Web应用
程序和API提供的一些资源。

- 应用程序安全需求  
为了创建一个安全的Web应用程序，您必须定义安全对该应用程序的意义。OWASP建议您使用《OWASP应用程序安全验证标准（ASVS）》作为您的应用设置安全需求的指导

- 应用程序安全架构  
与其费力去提升应用程序和接口的安全，不如在应用程序开发的初始阶段就进行安全设计，这样更能节约成本。作为好的开始，OWASP推荐[《OWASP Prevention Cheat Sheets》](https://owasp.org/www-project-cheat-sheets/)，用于指导如何在应用程序开发的初始阶段进行安全设计。

- 标准的安全控制  
建立强大并可用的安全控制极度困难。给开发人员提供一套标准的安全控制会极大简化应用程序和接口的安全开发过程。[“OWASP 主动控制” ](https://www.owasp.org/index.php/OWASP_Proactive_Controls)项目对开发者来说是个很好的开始。同时也有许多通用框架都已经有了授权、验证、CSRF等安全控制


- 安全开发生命周期  
为了改进企业遵循的应用程序开发流程，OWASP推荐使用[《OWASP软件保证成熟度模型(SAMM)》](https://owasp.org/www-project-samm/)。该模型能帮助企业组织制定并实施面对特定风险的软件安全战略  

- 应用程序安全教育   
OWASP教育项目为培训开发人员的web应用程序安全知识提供了培训材料。如果需要实际操作了解漏洞，可以使用OWASP webGoat、webGoat.NET、OWASP NodeJS Goat、OWASP Juice Shop Project或者OWASP Broken Web Applications Project。如果想了解最新资讯，请参加OWASP AppSec大会，OWASP 会议培训，或者本地的OWASP分部会议。

### 安全测试人员下一步做什么?  

**建立持续性的应用安全测试**  
安全编码很重要。但验证你想达到的安全性是否真实存在、是否正确、是否像我们想的那样也很关键。应用程序安全测试的目标是提供这些证据。这项工作困难而复杂，敏捷和DevOps当前快速发展的过程给传统的方法和工具带来的极大的挑战。因此，我们强烈建议你思考如何专注于整个应用程序组合中重要的地方，并且低成本高收益。


当前安全风险变化很快，每年进行一次的扫描或渗透测试的日子已经过去了。现代软件开发需要在整个软件开发生命周期中进行持续的应用安全测试。通过安全自动化来加强现有的开发管道并不会减缓开发速度。无论你选择哪种方法，都需考虑一下每年随着应用系统的规模倍增的定期测试、修复、复测并重新部署应用程序的成本。

- 理解威胁模型  
在你测试之前，请了解业务中需要耗时的重要部分。优先级来源于威胁模型，所以如果你还没有威胁模型，那么需要在测试开始前建立一个。考虑使用[《OWASP ASVS》](https://www.owasp.org/index.php/OWASP_Testing_Project)和[《OWASP安全测试指南》](https://www.owasp.org/index.php/OWASP_Testing_Project)作为指导标准，而不依赖工具厂商的结果来判断哪些是重要业务。

- 理解你的SDLC  
**你的应用安全测试方法必须与你们软件开发流程（SDLC）中的人员、工具和流程高度匹配**。试图强制推动额外的步骤、门限和审查可能会导致摩擦、绕行和一定范围内的争议。寻找自然而然的机会去收集安全信息，然后将融合进你的流程。

- 测试策略  
选择最简单、快速、准确的方法去验证每项需求。[《OWASP安全知识框架》](https://www.owasp.org/index.php/ASVS)和 [《OWASP应用程序安全验证标准》](https://www.owasp.org/index.php/ASVS)可以有助于您在单元测试或集成测试中做功能性或非功能性的安全测试。注意考虑用于工具误报的人力成本和漏报的严重危害。


- 实现全面性和准确性  
你不需要一切都要立刻测试。先关注那些重要的方面，然后随着时间扩展你的全面性。这意味着逐步扩展安全防御库和自动验证库，以及扩展应用系统和API本身的覆盖。目标是所有的应用程序和API基本安全性都能获得持续性的验证。

- 体现报告的价值  
**不管你测试得怎么专业，若不有效与别人沟通都等于白做**。展示你对程序运行的理解，从而建立互信关系。不必用晦涩难懂专业用语，清楚描述漏洞的滥用风险，然后在某场景下真实展现攻击。要对漏洞发现与利用难度及引发的后果做真实的评估。最后提交结果时请使用开发团队正在使用的文档工具格式，而不是简单的PDF。

### 企业组织下一步做什么? 
**现在就启动您的应用程序安全计划**  
应用程序安全已经不再是一个选择了。在日益增长的攻击和监管的压力下，企业组织必须建立一个有效的能力去确保应用程序和API的安全。由于在生产环境中的应用程序和APIs的代码行数惊人，许多企业组织都在努力处理数量巨大的漏洞。   
OWASP建议这些企业组织建立一个应用程序安全计划，深入了解并改善它们的应用程序组合的安全性。为了实现应用程序的安全性，需要企业组织中的不同部门之间有效地协同工作，这包括安全和审计、软件开发、商业和执行管理。安全应该可视化和可量化，让所有不同角色的人都可以看到并理解企业组织的应用程序的安全态势。通过消除或降低风险的方式专注于活动和结果，以帮助提高企业安全性。[《OWASP SAMM》](https://www.owasp.org/index.php/OWASP_SAMM_Project) 和首席信息官的[《OWASP应用安全指南》](https://www.owasp.org/index.php/Application_Security_Guide_For_CISOs)是这个列表中大多数关键活动的来源。

- 开始阶段  
 大型企业组织应在配置管理数据库（CMDB）中记录所有应用程序和相关数据资产。  
建立一个[应用程序安全计划](https://www.owasp.org/index.php/SAMM_-_Strategy_&_Metrics_-_1)并被采纳。  
进行[能力差距分析以比较您的组织和您的行业](https://www.owasp.org/index.php/SAMM_-_Strategy_&_Metrics_-_3)，从而定义重点改善的领域和执行计划。  
得到管理层的批准，并建立针对整个IT组织的[应用程序安全意识宣传活动](https://www.owasp.org/index.php/SAMM_-_Education_&_Guidance_-_1)。

- 基于风险的组合方法  
从业务角度识别[应用程序组合](https://www.owasp.org/index.php/SAMM_-_Strategy_&_Metrics_-_2)的保护需求。这一定程度上应该受到隐私法和与数据资产有关的其他条例的保护。   
建立一个[通用的风险等级模型](https://www.owasp.org/index.php/OWASP_Risk_Rating_Methodology)，该模型中的可能性和影响要素应该与组织风险承受能力一致的。   
相应的，度量并优先处理所有应用程序和 API。将结果添加到CMDB中。  
建立保证准则，合理定义所需的覆盖范围和级别。  

- 建立雄厚的基础  
建立一套集中关注的[策略和标准](https://www.owasp.org/index.php/SAMM_-_Policy_&_Compliance_-_2)，用于提供所有开发团队所遵循的一个应用程序安全基线。  
定义一组[通用的可重复使用的安全控制](https://www.owasp.org/index.php/OWASP_Security_Knowledge_Framework)，用于补充这些政策和标准，并提供使用它们的设计和开发指南。  
建立一个[应用程序安全培训课程](https://www.owasp.org/index.php/SAMM_-_Education_&_Guidance_-_2)，此课程应该要求所有的开发人员参加，并且针对不同的开发责任和主题进行修改。  

- 将安全整合入现有流程   
定义并集成[安全实施](https://www.owasp.org/index.php/SAMM_-_Construction)和[认证](https://www.owasp.org/index.php/SAMM_-_Verification)活动到现有的开发与操作流程之中。这些活动包括了[威胁建模](https://www.owasp.org/index.php/SAMM_-_Threat_Assessment_-_1)、安全设计和[审查](https://www.owasp.org/index.php/SAMM_-_Design_Review_-_1)、安全编码、[代码审查](https://www.owasp.org/index.php/SAMM_-_Code_Review_-_1)、[渗透测试](https://www.owasp.org/index.php/SAMM_-_Security_Testing_-_1)、修复等。  
[为开发和项目团队提供主题专家和支持服务](https://www.owasp.org/index.php/SAMM_-_Education_&_Guidance_-_3)，以保证他们的工作顺利进行。  

- 提高管理层对安全的可见度  
通过度量进行管理。根据对获取的度量和分析数据决定改进和投资的方向。这些度量包括：遵循安全实践和活动、引入的漏洞、修复的漏洞、应用程序覆盖率、按类型和实例数量衡量缺陷密度等。  
对实现和核查活动进行数据分析，寻找根本原因和漏洞模式，以推动整个企业的战略和系统改进。从失败中汲取经验，并提供积极的激励措施来促进进步。


延伸阅读  

https://owasp.org/www-project-web-security-testing-guide/  

发展方向。在您的企业组织中，需重点关注如何将安全
成为组织文化的一部分
https://owasp.org/www-project-samm/

源代码扫描工具 
https://owasp.org/www-community/Source_Code_Analysis_Tools


csrf
https://owasp.org/www-community/attacks/csrf

iso27001
https://www.iso.org/isoiec-27001-information-security.html

漏洞列表 
https://cwe.mitre.org/data/definitions/22.html

https://www.nist.gov/cyberframework
SAST
https://owasp.org/www-community/Source_Code_Analysis_Tools 


https://owasp.org/www-community/Vulnerability_Scanning_Tools

弱密码列表
https://github.com/danielmiessler/SecLists/tree/master/Passwords

https://pages.nist.gov/800-63-3/sp800-63b.html


资源列表  密码，用户名，webshell
https://github.com/danielmiessler/SecLists

安全头
https://owasp.org/www-project-secure-headers/


组件版本管理 
http://www.mojohaus.org/versions-maven-plugin/
https://www.owasp.org/index.php/OWASP_Dependency_Check

应急响应计划
https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final

应用安全向导 ciso
https://www.owasp.org/index.php/Application_Security_Guide_For_CISOs


cros 
https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties
https://www.corben.io/advanced-cors-techniques/
