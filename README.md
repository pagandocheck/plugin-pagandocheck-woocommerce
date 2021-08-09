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
Para poder aceptar pagos con **Pagando**, es necesario configurar el usuario y la llave generados en la plataforma de **Pagando**.

### 5. Activar el método de pago.
En el menu lateral izquierdo, en la sección **Woocommerce**, seleccionar la opción **Ajustes**, en la opción **Pagos**, activar Pagando como método de pago. 

