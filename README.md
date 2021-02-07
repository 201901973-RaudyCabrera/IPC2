# IPC2

## ¿Qué es XML? 

XML significa Extensible Markup Language(e X extensible M arkup I dioma). Usted puede aprender a través de este sitio Tutorial XML 

XML está diseñado para transmitir y almacenar datos. 

XML es un conjunto de reglas para definir la semántica de etiquetas, estas etiquetas documentará divididos en muchas partes y estas partes que ser identificados. 

También es un lenguaje de metamarcado que define la sintaxis del lenguaje utilizado para definir otra, semántica, lenguaje de marcado con estructura de dominio específico. 






## DOM (Document Object Model) 

Los datos XML se analiza en un árbol en la memoria, que opera a través del árbol de manipular XML. 

El Modelo de Objetos del Documento, o «DOM» por sus siglas en inglés, es un lenguaje API del Consorcio World Wide Web (W3C) para acceder y modificar documentos XML. Una implementación del DOM presenta los documento XML como un árbol, o permite al código cliente construir dichas estructuras desde cero para luego darles acceso a la estructura a través de un conjunto de objetos que implementaron interfaces conocidas. 

El DOM es extremadamente útil para aplicaciones de acceso directo. SAX sólo te permite la vista de una parte del documento a la vez. Si estás mirando un elemento SAX, no tienes acceso a otro. Si estás viendo un nodo de texto, no tienes acceso al elemento contendor. Cuando desarrollas una aplicación SAX, necesitas registrar la posición de tu programa en el documento en algún lado de tu código. SAX no lo hace por ti. Además, desafortunadamente no podrás mirar hacia adelante (look ahead) en el documento XML. 

Algunas aplicaciones son imposibles en un modelo orientado a eventos sin acceso a un árbol. Por supuesto que puedes construir algún tipo de árbol por tu cuenta en eventos SAX, pero el DOM te evita escribir ese código. El DOM es una representación de árbol estándar para datos XML. 

Las aplicaciones DOM suelen comenzar analizando algún XML en un DOM. Con xml.dom.minidom, esto se hace a través de las funciones de análisis sintáctico: 

_from xml.dom.minidom import parse, parseString  
dom1 = parse('c:\\temp\\mydata.xml') # parse an XML file by name  
datasource = open('c:\\temp\\mydata.xml')  
dom2 = parse(datasource) # parse an open file  
dom3 = parseString('<myxml>Some data<empty/> some more data</myxml>')_

También puedes crear un objeto Document invocando a un método en un objeto de la «Implementación del DOM». Puedes obtener este objeto llamando a la función getDOMImplementation() del paquete xml.dom o del módulo xml.dom.minidom. Una vez que tengas un objeto Document, puedes agregarle nodos secundarios para llenar el DOM:

_from xml.dom.minidom import getDOMImplementation  
impl = getDOMImplementation()  
newdoc = impl.createDocument(None, "some_tag", None)  
top_element = newdoc.documentElement  
text = newdoc.createTextNode('Some textual content.')  
top_element.appendChild(text)_

Puedes evitar invocar este método explícitamente utilizando la declaración with. El siguiente código desvinculará automáticamente dom cuando se salga del bloque with: 

_with xml.dom.minidom.parse(datasource) as dom: ... # Work with dom._

La propiedad principal del objeto del documento es documentElement. Te proporciona el elemento principal en el documento XML: el que contiene a todos los demás. Aquí hay un programa de ejemplo: 

_dom3 = parseString("<myxml>Some data</myxml>")  
assert dom3.documentElement.tagName == "myxml"_

Este programa de ejemplo es una demostración bastante realista de un programa simple. En este caso particular, no aprovechamos mucho la flexibilidad del DOM. 


import xml.dom.minidom
document = """\
<slideshow> 
<title>Demo slideshow</title> 
<slide><title>Slide title</title> 
<point>This is a demo</point> 
<point>Of a program for processing slides</point> 
</slide> 
 
<slide><title>Another demo slide</title> 
<point>It is important</point> 
<point>To have more than</point> 
<point>one slide</point> 
</slide> 
</slideshow> 
""" 
 
dom = xml.dom.minidom.parseString(document) 
 
def getText(nodelist): 
rc = [] 
for node in nodelist: 
if node.nodeType == node.TEXT_NODE: 
rc.append(node.data) 
return ''.join(rc) 
 
def handleSlideshow(slideshow): 
print("<html>") 
handleSlideshowTitle(slideshow.getElementsByTagName("title")[0]) 
slides = slideshow.getElementsByTagName("slide") 
handleToc(slides) 
handleSlides(slides) 
print("</html>") 
 
def handleSlides(slides): 
for slide in slides: 
handleSlide(slide) 
 
def handleSlide(slide): 
handleSlideTitle(slide.getElementsByTagName("title")[0]) 
handlePoints(slide.getElementsByTagName("point")) 
 
def handleSlideshowTitle(title): 
print("<title>%s</title>" % getText(title.childNodes))_






## Xpath (módulo python) 

Xpath es un módulo que es parte de la librería xml.etree.ElementTree por lo general la misma se importa de la siguiente manera: 

_import xml.etree.ElementTree as ET_

Xpath provee una serie de expresiones para localizar elementos en un árbol, su finalidad es proporcionar un conjunto de sintaxis, por lo que debido a su limitado alcance no se considera un motor en si mismo. 

_import xml.etree.ElementTree as ET 
root = ET.fromstring(docxml) 
 - Elementos de nivel superior 
root.findall(".") 
 - todos los hijos de neighbor o nietos de country en el nivel superior 
root.findall("./country/neighbor") 
 - Nodos xml con name='Singapore' que sean hijos de 'year' 
root.findall(".//year/..[@name='Singapore']") 
 - nodos 'year' que son hijos de etiquetas xml cuyo name='Singapore' 
root.findall(".//*[@name='Singapore']/year") 
 - todos los nodos 'neighbor'que son el segundo hijo de su padre
 root.findall(".//neighbor[2]")_

Ejemplo de uso de Xpath dentro de una sentencia xml: 

_<xpath expr="//field[@name]='is_done'" position="before">
  <field name="date_deadline" /> 
</xpath>_

La biblioteca ElementTree incluye herramientas para analizar XML usando APIs basadas en eventos y documentos, buscando documentos analizados con expresiones XPath, y creando nuevos o modificando documentos existentes.

Estos ejemplos se hacen sobre el fichero `book.xml`:

	<?xml version="1.0" encoding="utf-8"?>
	<bookstore>
	<book category="COOKING">
	  <title lang="en">Everyday Italian</title>
	  <author>Giada De Laurentiis</author>
	  <year>2005</year>
	  <price>30.00</price>
	</book>
	<book category="CHILDREN">
	  <title lang="en">Harry Potter</title>
	  <author>J K. Rowling</author>
	  <author>Giada De Laurentiis</author>
	  <year>2006</year>
	  <price>29.99</price>
	</book>
	</bookstore>
  
Todas las búsquedas se hacen desde el nodo raíz.

1. Ruta de localización:

	* `/bookstore/book`: Selecciona todos los nodos "book" que están en la ruta.
	* `//year`: Selecciona todos los nodos "year" a partir del nodo raíz.
	* `/bookstore/book/title/text()`: Selecciona todos los nodos texto (información) de los elementos seleccionados con la ruta.
	* `/bookstore/book/title/@lang`: Selecciona todos los atributos llamados "lang" de los elementos seleccionados con la ruta.
	* `.` : Selecciona el nodo actual.
	* `..` : Selecciona al nodo padre.
	* `*` : Selecciona todos los nodos

2. Filtrado de nodos:

	* `/bookstore/book[title="Everyday Italian"]`: Predicado que filtra de todos los nodos seleccionados con la ruta, aquellos que tienen un nodo hijo cuya información coincide con la indicada.
	* `/bookstore/book[year>2005]`: En este caso se hace una comparación numérica.
	* `/bookstore/book/title[@lang="en"]`: Ahora la selección se hace con un atributo.
	* `/bookstore/book/title[@lang="en"]/text()`: Ejemplo igual que el anterior, pero en este caso se selecciona el nodo texto (información).
	* `//book[@category="CHILDREN" and price="29.99"]`: Se pueden utilizar expresiones lógicas: and y or.

3. Algunas funciones xpath

	* `count(book/title)`: Devuelve el número de nodos seleccionados.
	* `sum(//book/price)`: Devuelve la suma de los valores de los nodos seleccionados.
	* `//book/author[contains(text(),'De')]`: Devuelve los autores cuya información contine la subcadena "De".

