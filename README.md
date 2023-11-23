# Ajax
Hacemos un `Bundle install` y `rails db:migrate`, luego de eso corregimos algunos errores que trae el proyecto y luego de dejarlo funconal hacemos un `rails server` para comenzar a trabajar en las demas partes.
![](/imagenes/1.png)
## Parte 1
**¿Cómo sabe la acción de controlador si show fue llamada desde código JavaScript o mediante una petición HTTP normal iniciada por el usuario?**  
La acción del controlador verifica si fue llamada a través de AJAX utilizando `request.xhr?`. Este método devuelve `true` si la solicitud proviene de código JavaScript (AJAX), y `false` si es una solicitud HTTP normal iniciada por el usuario. El controlador ajusta su comportamiento en consecuencia, renderizando una vista parcial para solicitudes AJAX y una vista completa para solictudes normales.
## Parte 2
**¿Cómo debería construir y lanzar la petición XHR el código JavaScript?**  
Para construir y lanzar la petición XHR en el código JavaScript, puedes utilizar el objeto `XMLHttpRequest` o, en este caso, el método más moderno utilizando `$.ajax` de jQuery. El códgo se encarga de mostrar información detallada de una película en una ventana emergente cuando se hace clic en un enlace que lleva al detalle de esa película.

**Explica el siguiente código**

1. **Configuración inicial:**
   ```javascript
   setup: function() {
     let popupDiv = $('<div id="movieInfo"></div>');
     popupDiv.hide().appendTo($('body'));
     $(document).on('click', '#movies a', MoviePopup.getMovieInfo);
   },
   ```
   - Se crea un elemento `div` oculto (`popupDiv`) que actuará como la ventana emergente.
   - Este elemento se agrega al final del cuerpo (`body`) del documento pero permanece oculto.
   - Se establece un evento de clic en los enlaces dentro de los elementos con id `#movies` para llamar a la función `getMovieInfo`.

2. **Obtención de información de la película:**
   ```javascript
   ,getMovieInfo: function() {
     $.ajax({
       type: 'GET',
       url: $(this).attr('href'),
       timeout: 5000,
       success: MoviePopup.showMovieInfo,
       error: function(xhrObj, textStatus, exception) { alert('Error!'); }
     });
     return(false);
   },
   ```
   - Se realiza una solicitud AJAX (`$.ajax`) al enlace (`href`) del elemento en el que se hizo clic.
   - Si la solcitud tiene éxito, se llama a la función `showMovieInfo`.
   - Si hay un error, se muestra una alerta.

3. **Mostrar información de la película:**
   ```javascript
   ,showMovieInfo: function(data, requestStatus, xhrObject) {
     let oneFourth = Math.ceil($(window).width() / 4);
     $('#movieInfo').
       css({'left': oneFourth,  'width': 2*oneFourth, 'top': 250}).
       html(data).
       show();
     $('#closeLink').click(MoviePopup.hideMovieInfo);
     return(false);
   },
   ```
   - Se calcula el ancho de la ventana emergente (`oneFourth`).
   - Se establece la posición y el tamaño de la vntana emergente usando CSS y se muestra con la información de la película.
   - Se establece un evento de clic en el enlace de cierre (`#closeLink`) para ocultar la ventana emergente.

4. **Ocultar información de la película:**
   ```javascript
   ,hideMovieInfo: function() {
     $('#movieInfo').hide();
     return(false);
   }
   ```
   - Oculta la ventana emergente cuando se hace clic en el enlace de cierre (`#closeLink`).

**¿Cuáles son tus resultados?**
- Al hacer clic en un enlace dentro de `#movies`, se debería mostrar una ventana emergente con información detallada de la película.
- La ventana emergente debería flotar en la pantalla según las propiedades CSS especificadas.
- Se ha aplicado un truco de CSS para estilizr la ventana emergente (`#movieInfo`) con bordes, fondo y posición absoluta.
## Parte 3
**¿Cuál es solución que brinda jQuery a este problema?**
jQuery brinda una solución a este problema mediante el uso de la delegación de eventos de esta manera:
```javascript
$(document).on('click', '.myClass', func);
```
