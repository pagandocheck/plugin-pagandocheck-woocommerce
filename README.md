# plugin-pagandocheck-woocommerce
Plugin de Woocommerce para pagos con Pagando Check.


## Instalar plugin de Pagando para pagos en la tienda.

## Requisitos previos

  - Tener una **cuenta** de **empresa** en **Pagando Check**.
  - Tener una tienda de Woocommerce, si no la tiene, puede instalar nuestra tienda demo siguiendo las instrucciones de este [repositorio](https://github.com/pagandocheck/woocommerce-store).
  - Tener permisos de administrador o poder realizar modificaciones o configuraciones del módulo de **Pagando Check**.
  - Ya que el módulo de **Pagando Check** se **utiliza** para que un **sitio web externo pueda procesar pagos**,
  se recomienda que este tipo de pago lo configure una persona con conocimientos técnicos.

## 1. Crear un usuario administrador.
Para poder generar un nuevo usuario administrador, hay que agregarlo mediante la herramienta de MAMP llamada phpMyAdmin, que anteriormente se usó en la instalaciónn de la tienda demo. Seleccionar la base de datos ```woocommerce ``` y en la seccion ```SQL```,pegar la siguiente connsulta:

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

## 2. Iniciar sesión en Wordpress
Ingresar a la página de configuración de Wordpress de nuestra tienda **http://localhost/woocommerce-store/wp-login.php** e iniciar sesión con las credenciales que se configuraron en el paso anterior, o con nuestras credenciales default:

```
user: demo
password: demo

```

Se mostrará la página de Wordpress instalada localmente, en la cual se pueden configurar las diferentes opciones de la tienda de Woocommerce, incluyendo los productos, precios y métodos de pago.

## 3. Agregar plugin de Pagando.
Para obtener el modulo debe clonar este proyecto de github en su equipo de computo con el siguiente comando:

```
git clone git@github.com:pagandocheck/plugin-pagandocheck-woocommerce.git
```

Una vez descargado hay que comprimir la carpeta **plugin-pagandocheck-woocommerce**, despues en menu lateral izquierdo del panel de administración de la tienda, en la sección **Plugins**, seleccionar la opción **Add plugin** y despues en la opción **upload plugin**, subir la carpeta comprimida (.zip) con el plugin de **Pagando Check**.

## 4. Activar plugin
Para activar el plugin , ir al menu lateral izquierdo, en la sección **Plugins > Installed Plugins**, y seleccionar **Activate** en el plugin de **Pagando Check**. Cuando se ha realizado correctamente , deberá aparecer **Pagando** en el menú lateral izquierdo.

## 5. Generar llaves de prueba.
Para poder aceptar pagos con **Pagando Check**, es necesario configurar un usuario y una llave de prueba.

Para obtener sus llaves de prueba debe ingresar con su cuenta empresarial a https://negocios.pagando.mx

<img width="1266" alt="Captura de Pantalla 2021-08-11 a la(s) 13 40 38" src="https://user-images.githubusercontent.com/88348069/129092607-1e4b96f6-cd8e-4538-a9e0-d2094361eb47.png">

Una vez dentro, en el menú de opciones, dentro del apartado de pagos, ingresara a **API para sitio web**. Y luego hacer clic en **Botón Checkout**.

<img width="784" alt="Captura de Pantalla 2021-08-11 a la(s) 13 44 18" src="https://user-images.githubusercontent.com/88348069/129093055-57741a7a-3a67-4da6-a13b-0ca99a83fdf3.png">

Depués en la opción **Woocommerce**, en la primera sección, podrá generar y recuperar sus llaves de prueba.

<img width="779" alt="testing-keys" src="https://user-images.githubusercontent.com/88348069/135500623-f05a1655-3dcc-4df9-89bb-74f81006cc44.png" style="display: block; margin-left: auto; margin-right: auto;">

### Configuración de módulo Checkout

Aquí se configura la dirección a donde quiere regresar a sus clientes una vez que se ha efectuado el pago, entre otras configuraciones.

<img width="816" alt="redirect-config" src="https://user-images.githubusercontent.com/88348069/135501874-3a18fa09-f7d5-4100-b9be-bed71d3436a8.png" style="display: block; margin-left: auto; margin-right: auto;">

### Agregar credenciales en la tienda

Para agregar las credenciales generadas previamente, es necesario ingresar al panel de administración de la tienda (http://localhost/woocommerce-store/wp-admin/admin.php) y agregarlas accediendo al menu lateral izquierdo **Pagando**.

<img width="813" alt="Captura de Pantalla 2021-08-20 a la(s) 10 07 25" src="https://user-images.githubusercontent.com/88348069/130254111-d53c7c1e-9a38-4e41-a0ce-86eeeea7862d.png">


## 6. Activar el método de pago.
En el menu lateral izquierdo, en la sección **Woocommerce**, seleccionar la opción **Settings**, en la opción **Payments**, activar **Pagar con tu cuenta Pagando** y **Pagar como invitado**. 

<img width="1429" alt="Captura de Pantalla 2021-08-20 a la(s) 10 44 20" src="https://user-images.githubusercontent.com/88348069/130259008-bd272d42-df8d-4596-a5c8-2b344235d370.png">
