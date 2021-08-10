# plugin-pagandocheck-woocommerce
Plugin de Woocommerce para pagos con Pagando Check.


## Instalar plugin de Pagando para pagos en la tienda.

### 1. Crear un usuario administrador.
Para poder generar un usuario administrador, hay que agregarlo mediante la herramienta de MAMP llamada phpMyAdmin que anteriormente se usó. Seleccionar la base de datos ```woocommerce ``` y en la seccion ```SQL```,pegar la siguiente connsulta:

```
-- Cambiar valores
START TRANSACTION;

SET @prefix = 'wp_';
SET @user_login = 'demo';
SET @user_pass = 'demo';
SET @user_email = 'demo@demo.com';
SET @display_name = 'Nombre Demo';

-- Actualiza tabla de usuarios
SET @field_values_user = '(`user_login`, `user_pass`, `user_email`, `display_name`, `user_registered`) VALUES (@user_login, MD5(@user_pass), @user_email, @display_name, NOW())';

SET @user = CONCAT("INSERT INTO ", @prefix, "users ", @field_values_user);

PREPARE tbluser FROM @user;
EXECUTE tbluser;
DEALLOCATE PREPARE tbluser;
 
-- Actualiza tabla usermeta
SET @id_user = LAST_INSERT_ID();

SET @field_values_meta =CONCAT('(`user_id`, `meta_key`, `meta_value`) 
VALUES (@id_user, "', @prefix, 'capabilities",''a:1:{s:13:"administrator";b:1;}'');');

SET @usermeta = CONCAT("INSERT INTO ", @prefix, "usermeta ", @field_values_meta);

PREPARE tblmeta FROM @usermeta;
EXECUTE tblmeta;
DEALLOCATE PREPARE tblmeta;

-- compatibilidad
SET @field_values_meta =CONCAT('(`user_id`, `meta_key`, `meta_value`) 
VALUES (@id_user, "',@prefix, 'user_level", "10");');

SET @usermeta = CONCAT("INSERT INTO ", @prefix, "usermeta ", @field_values_meta);

PREPARE tblmeta FROM @usermeta;
EXECUTE tblmeta;
DEALLOCATE PREPARE tblmeta;

COMMIT;

```

> **_Nota:_**
Lo único que se tiene que cambiar son las siguientes variables:

```
SET @user_login = 'demo';
SET @user_pass = 'demo';
SET @user_email = 'demo@demo.com';
SET @display_name = 'Nombre Demo';
```

### 2. Iniciar sesión en Wordpress
Ingresar a la página de configuración de Wordpress de nuestra tienda **http://localhost/woocommerce-store/wp-login.php** e iniciar sesión con las credenciales que se configuraron en el paso anterior.

Se mostrará la página de Wordpress instalada localmente, en la cual se pueden configurar las diferentes opciones de la tienda de Woocommerce, incluyendo los productos, precios y métodos de pago.

### 3. Agregar plugin de Pagando.
Ir al menu lateral izquierdo, en la sección **Plugins**, seleccionar la opción **Agregar plugin** y despues en la opción **cargar plugin**, subir la carpeta comprimida (.zip) con el plugin de Pagando.

### 3. Activar plugin
Para activar el plugin , ir al menu lateral izquierdo, en la sección **Plugins**, y seleccionar **Activar** en el plugin de Pagando. Cuando se ha realizado correctamente , deberá aparecer **Pagando** en el menú lateral izquierdo.

### 4. Agregar credenciales.
Para poder aceptar pagos con **Pagando Check**, es necesario configurar el usuario y la llave de prueba generados en la plataforma de **Pagando**.

En el panel de administración ,seleccionar la opción **Api para sitio web**. En la siguiente sección **"Genera tu llave de Pruebas"** incluye un usuario y la opción de recuperar una clave pública para la prueba, si lo desea, es posible generar una nueva clave cada vez que lo desee por seguridad.

-**Consultar (Obtener clave) .-** Haga clic en "Consultar" para obtener la ** clave pública ** para las pruebas.

-**Usuario (Usuario) .-** Este es el identificador de su empresa para Pagando Check API®, esta es la forma en que Pagando reconoce las transacciones de los pagos de su cuenta.

-**Clave pública para pruebas .-** Esta es la contraseña para obtener un token de acceso, con este token se podría configurar el plugin de woocommerce.

<img width="1670" alt="Captura_de_pantalla_2020-07-17_a_la(s)_14 20 26" src="https://user-images.githubusercontent.com/88348069/128936180-3b6237aa-b814-4e73-a622-13bcbb392394.png">

### 5. Activar el método de pago.
En el menu lateral izquierdo, en la sección **Woocommerce**, seleccionar la opción **Ajustes**, en la opción **Pagos**, activar **Pagar con tu cuenta Pagando**. 

