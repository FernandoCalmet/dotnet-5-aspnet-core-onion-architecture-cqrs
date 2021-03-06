# 馃 C# ASP.NET CORE 5 ONION ARCHITECTURE CQRS

[![Github][github-shield]][github-url]
[![Kofi][kofi-shield]][kofi-url]
[![LinkedIn][linkedin-shield]][linkedin-url]
[![Khanakat][khanakat-shield]][khanakat-url]

## TABLA DE CONTENIDO

* [Acerca del proyecto](#acerca-del-proyecto)
* [Instalaci贸n](#instalaci贸n)
* [Resumen te贸rico](#resumen-te贸rico)
* [Dependencias](#dependencias)
* [Licencia](#licencia)

## 馃敟 ACERCA DEL PROYECTO

Este proyecto es una muestra de un esqueleto de aplicaci贸n con arquitectura de cebolla y patr贸n CQRS. Se utilizo ``ASP.NET Core 5`` con C#.

## 鈿欙笍 INSTALACI脫N

Clonar el repositorio.

```bash
gh repo clone FernandoCalmet/dotnet-5-aspnet-core-onion-architecture-cqrs
```

Ejecutar aplicaci贸n.

```bash
dotnet run
```

## 馃摀 RESUMEN TE脫RICO

### La Necesidad De Seguir Una Arquitectura

Para mantener la cordura estructural en soluciones medianas a grandes, siempre se recomienda seguir alg煤n tipo de arquitectura. Debe haber visto que la mayor铆a de los proyectos de c贸digo abierto tienen m煤ltiples capas de proyectos dentro de una estructura de carpetas compleja.

### Capa Vs Niveles

Cuando solo hay una separaci贸n l贸gica en su aplicaci贸n, podemos denominarla capas o N capas. En los casos en los que existe una separaci贸n f铆sica y l贸gica de preocupaciones, a menudo se la denomina aplicaci贸n de n niveles, donde n es el n煤mero de separaciones. 3 es el valor m谩s com煤n de N. En este proyecto se implementara la Arquitectura en capas.

Esta estratificaci贸n puede ayudar en la separaci贸n de preocupaciones, subdividiendo la soluci贸n en unidades m谩s peque帽as para que cada unidad sea responsable de una tarea espec铆fica y tambi茅n para aprovechar la abstracci贸n. Para proyectos de escala media a grande donde trabajan varios equipos, las capas tienen ventajas muy obvias bajo la manga. Permite que un equipo o individuo espec铆fico trabaje en una capa en particular sin perturbar la integridad de los dem谩s. Hace que sea mucho m谩s f谩cil realizar un seguimiento de los cambios mediante el control de fuente.

Adem谩s, hace que toda su soluci贸n se vea limpia.

### Breve Descripci贸n General De La Arquitectura De N-Layer

Una de las arquitecturas m谩s populares en aplicaciones ASP.NET Core. Aqu铆 hay una representaci贸n esquem谩tica simple de una variaci贸n de la Arquitectura de N capas. La capa de presentaci贸n normalmente contiene la parte con la que el usuario puede interactuar, es decir, WebApi, MVC, formularios web, etc. La l贸gica empresarial es probablemente la parte m谩s importante de toda esta configuraci贸n. Contiene todas las l贸gicas relacionadas con el requisito empresarial. Ahora, idealmente, cada aplicaci贸n tiene su propia base de datos dedicada. Para acceder a la base de datos, introducimos una capa de acceso a datos. Esta capa generalmente contiene ORM para que ASP.NET obtenga/escriba en la base de datos.

![Layered](.img/layered.png)

### Desventajas De La Arquitectura De N Capas

Para comprender claramente las ventajas de la arquitectura Onion en las aplicaciones ASP.NET Core, necesitaremos estudiar los problemas con la arquitectura N Layer. Es una de las arquitecturas de soluciones m谩s utilizadas entre los desarrolladores de .NET.

En lugar de construir una estructura altamente desacoplada, a menudo terminamos con varias capas que dependen unas de otras. Esto es algo realmente malo en la creaci贸n de aplicaciones escalables y puede plantear problemas con el crecimiento de la base de c贸digo. Para dejarlo claro, en el diagrama anterior podemos ver que la capa de presentaci贸n depende de la capa de l贸gica, que a su vez depende del acceso a los datos y as铆 sucesivamente.

Por lo tanto, estar铆amos creando un mont贸n de acoplamientos innecesarios. 驴Es realmente necesario? En la mayor铆a de los casos, la capa de interfaz de usuario (presentaci贸n) tambi茅n se acoplar铆a a las capas de acceso a datos. Esto frustrar铆a el prop贸sito de tener una arquitectura limpia.

En la Arquitectura de N capas, la base de datos suele ser el n煤cleo de toda la aplicaci贸n, es decir, es la 煤nica capa que no tiene que depender de nada m谩s. Cualquier peque帽o cambio en la capa de l贸gica empresarial o en la capa de acceso a datos puede resultar peligroso para la integridad de toda la aplicaci贸n.

### Introducci贸n A La Arquitectura Onion

La arquitectura Onion, presentada por Jeffrey Palermo, supera los problemas de la arquitectura en capas con gran facilidad. Con Onion Architecture, el cambio de juego es que la capa de dominio (entidades y reglas de validaci贸n que son comunes al caso comercial) est谩 en el n煤cleo de toda la aplicaci贸n. Esto significa mayor flexibilidad y menor acoplamiento. En este enfoque, podemos ver que todas las capas dependen solo de las capas principales.

![Layered](.img/onion.png)

As铆 es como desglosar铆a la estructura de la Soluci贸n propuesta.

**La capa de dominio y aplicaci贸n** estar谩 en el centro del dise帽o. Podemos referirnos a estas capas en Core Layers. Estas capas no depender谩n de ninguna otra capa.

La capa de dominio generalmente contiene entidades y l贸gica empresarial. La capa de aplicaci贸n tendr铆a interfaces y tipos. La principal diferencia es que la capa de dominio tendr谩 los tipos que son comunes a toda la empresa, por lo que tambi茅n se puede compartir con otras soluciones. Pero la capa de aplicaci贸n tiene tipos e interfaces espec铆ficos de la aplicaci贸n. 驴Comprender?

Como se mencion贸 anteriormente, las capas principales nunca depender谩n de ninguna otra capa. Por lo tanto, lo que hacemos es crear interfaces en la capa de aplicaci贸n y estas interfaces se implementan en las capas externas. Esto tambi茅n se conoce como Principio de Inversi贸n de Dependencia o DIP.

Por ejemplo, si su aplicaci贸n quiere enviar un correo, definimos un IMailService en la capa de aplicaci贸n y lo implementamos fuera de las capas principales. Con DIP, es posible cambiar f谩cilmente las implementaciones. Esto ayuda a crear aplicaciones escalables.

**La capa de presentaci贸n** es donde idealmente desear铆a colocar el proyecto al que el usuario puede acceder. Puede ser un proyecto WebApi, Mvc, etc.

**La capa de infraestructura** es un poco m谩s complicada. Es donde le gustar铆a agregar su infraestructura. La infraestructura puede ser cualquier cosa. Tal vez una capa principal de Entity Framework para acceder a la base de datos, o una capa creada espec铆ficamente para generar tokens JWT para la autenticaci贸n o incluso una capa Hangfire. Comprender谩 m谩s cuando comencemos a implementar la arquitectura Onion en ASP.NET Core WebApi Project.

## 馃摜 DEPENDENCIAS

- [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) : Herramientas Swagger para documentar API creadas en ASP.NET Core.

## 馃搫 LICENCIA

Este proyecto est谩 bajo la Licencia (Licencia MIT) - mire el archivo [LICENSE](LICENSE) para m谩s detalles.

## 猸愶笍 DAME UNA ESTRELLA

Si esta Implementaci贸n le result贸 煤til o la utiliz贸 en sus Proyectos, d茅le una estrella. 隆Gracias! O, si te sientes realmente generoso, [隆Apoye el proyecto con una peque帽a contribuci贸n!](https://ko-fi.com/fernandocalmet).

<!--- reference style links --->
[github-shield]: https://img.shields.io/badge/-@fernandocalmet-%23181717?style=flat-square&logo=github
[github-url]: https://github.com/fernandocalmet
[kofi-shield]: https://img.shields.io/badge/-@fernandocalmet-%231DA1F2?style=flat-square&logo=kofi&logoColor=ff5f5f
[kofi-url]: https://ko-fi.com/fernandocalmet
[linkedin-shield]: https://img.shields.io/badge/-fernandocalmet-blue?style=flat-square&logo=Linkedin&logoColor=white&link=https://www.linkedin.com/in/fernandocalmet
[linkedin-url]: https://www.linkedin.com/in/fernandocalmet
[khanakat-shield]: https://img.shields.io/badge/khanakat.com-brightgreen?style=flat-square
[khanakat-url]: https://khanakat.com
