# oidc-microservice-architecture-project with thymeleaf
#### Проектот е направен од Марио Петковски и Бисера Стојмановска

<hr>


![alt text](https://github.com/mariopetkovskii/oidc-microservice-architecture-project/blob/master/Diagram.png?raw=true)

<hr>


	1. Секој корисникот може да пристапи до http:// localhost:9091/index тоа е почетна страна која не бара автентикација на кориникот и е достапна за сите.
	2. Серверот враќа HTML кој се рендерира на страната.
	3. Корисникот се обидува да пристапи до http://localhost:9091/cars каде што е потребна автентикација за да ја види страната.
	4. Корисникот е пренасочен од прелистувачот кон серверот на Keycloack каде што мора да се најави на својот account.
	5. Прелистувачот испраќа барање до серверот Keycloack за да ја прикаже формата каде ќе се најави корисникот.
	6. Барањето е одобрено па на browser-от преку Keycloack се прикажува формата за најава.
	7. Корисникот го внесува своето корисничко име и лозинка за да се најави преку Keycloack-POST барање.
	8. Keycloack го потврдува идентитетот на корисникот и прави пренасочување на корисникот до апликацијата.
	9. Прелистувачот праќа GET барање до sso/login кој е поставен во keycloack адаптер.
	10. Апликацијата испраќа GET барање на серверот и очекува да се врати токенот за автентикација на корисникот.
	11. Апликацијата го зема JWT и отвора нова сесија за корисникот.
	12. Апликацијата пренасочува до заштитениот ресурс побаран од корисникот како cookie.
	13. Прелистувачот повторно го бара ресурсот заштитен со испраќање на SessionID
	14. Апликацијата врши авторизација односно проверува дали улогата на корисникот дозволува пристап до овој ресурс и го испраќа назад.
	15. Автентицираниот корисник бара да пристапи до /manufacturers.
	16. Апликацијата го испраќа барањето до микросервисот за manufacturers со испраќање на JWT користејќи KeyCloack RestTemplate.
	17. За да ја провери валидноста на JWT, на микросервисот му е потребен јавниот клуч па затоа испраќа барање до серверот на Keycloack.
	18. Серверот Keycloack ги обезбедува јавен клуч.
	19. Микросервисот го враќа JSON.
	20. Апликацијата го враќа резултатот во апликацијата се рендерира соодветен HTML.
	
	JWT содржи информации потребни за директен пристап до ресурс односно се користат информациите во токенот за да одлучи дали клиентот е овластен  или не. 
	
	• RefreshToken: Секогаш кога е потребен токен за пристап за пристап до одреден ресурс, клиентот може да го користи токенот за освежување за да добие нов токен за пристап издаден од Keycloak. Тоа се случува кога: токенот ќе истече или кога пристапуваме до нов ресурс за прв пат.
