# Escuela Colombiana de Ingeniería
# Arquitecturas de Software - ARSW
### Taller – Principio de Inversión de dependencias, Contenedores Livianos e Inyección de dependencias.
#### Integrantes: Julia Mejia y Cristian Rodriguez  

Parte I. Ejercicio básico.

Para ilustrar el uso del framework Spring, y el ambiente de desarrollo para el uso del mismo a través de Maven (y NetBeans), se hará la configuración de una aplicación de análisis de textos, que hace uso de un verificador gramatical que requiere de un corrector ortográfico. A dicho verificador gramatical se le inyectará, en tiempo de ejecución, el corrector ortográfico que se requiera (por ahora, hay dos disponibles: inglés y español).

1. Abra el los fuentes del proyecto en NetBeans.

2. Revise el archivo de configuración de Spring ya incluido en el proyecto (src/main/resources). El mismo indica que Spring buscará automáticamente los 'Beans' disponibles en el paquete indicado.

3. Haciendo uso de la [configuración de Spring basada en anotaciones](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-spring-beans-and-dependency-injection.html) marque con las anotaciones @Autowired y @Service las dependencias que deben inyectarse, y los 'beans' candidatos a ser inyectadas -respectivamente-:

	* GrammarChecker será un bean, que tiene como dependencia algo de tipo 'SpellChecker'.  
	En la clase GrammarChecker añadimos la anotacion @Service y @Autowired para la dependencia 

		```java
		@Service
		public class GrammarChecker {
		@Autowired
		SpellChecker sc;
		String x; ...
		```

	* EnglishSpellChecker y SpanishSpellChecker son los dos posibles candidatos a ser inyectados. Se debe seleccionar uno, u otro, mas NO ambos (habría conflicto de resolución de dependencias). Por ahora haga que se use EnglishSpellChecker.  
	En la clase EnglishSpellChecker agregamos la anotacion @Service

		```java
		@Service
		public class EnglishSpellChecker implements SpellChecker {
			@Override
			public String checkSpell(String text) {		
			return "Checked with english checker:"+text;
			}
		}
		```
		
 
4.	Haga un programa de prueba, donde se cree una instancia de GrammarChecker mediante Spring, y se haga uso de la misma:

	```java
	public static void main(String[] args) {
		ApplicationContext ac=new ClassPathXmlApplicationContext("applicationContext.xml");
		GrammarChecker gc=ac.getBean(GrammarChecker.class);
		System.out.println(gc.check("la la la "));
	}
	```

 	<img width="908" alt="image" src="https://github.com/juliamejia/Spring_LightweightCont_Annotation-DI_Example/assets/98657146/29ee2d32-4474-4a50-837d-34e9bc18f17d">  

	
5.	Modifique la configuración con anotaciones para que el Bean ‘GrammarChecker‘ ahora haga uso del  la clase SpanishSpellChecker (para que a GrammarChecker se le inyecte EnglishSpellChecker en lugar de  SpanishSpellChecker. Verifique el nuevo resultado.  
   	En la clase SpanishSpellChecker se agrega la etiqueta @Service y se quita de la otra clase  
  	
  	```java
	@Service
	public class SpanishSpellChecker implements SpellChecker {
		@Override
		public String checkSpell(String text) {
		return "revisando ("+text+") con el verificador de sintaxis del espanol";  
		}
	}
	```

   	<img width="835" alt="image" src="https://github.com/juliamejia/Spring_LightweightCont_Annotation-DI_Example/assets/98657146/08acd07b-7b24-4a83-997b-9101c42ab47b">


  	
