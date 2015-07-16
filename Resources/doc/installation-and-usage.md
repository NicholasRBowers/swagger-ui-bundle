Swagger UI Bundle
=================

## Installation & Usage

Install via Composer:

`$ composer require activelamp/swagger-ui-bundle`

Enable in `app/AppKernel.php`:

```php
<?php

use Symfony\Component\HttpKernel\Kernel;
use Symfony\Component\Config\Loader\LoaderInterface;

class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            ...
            new ActiveLAMP\Bundle\SwaggerUIBundle\ALSwaggerUIBundle(),

```
## Map Resource Lists

The application needs to know where to find your API's resource-list JSON or YAML. The `resource_list` option can receive 2 types of values:

1. An absolute URL to an external Swagger resource-list.
2. A route name.

#### Example 1 (externally hosted resource list):
```
# app/config/config.yml

al_swagger_ui:
    resource_list: http://petstore.swagger.wordnik.com/api/api-docs
```

And add the route to `app/config/routing.yml`:

```
al_swagger_ui:
    resource: @ALSwaggerUIBundle/Resources/config/routing.yml
    prefix: /docs
```

The swagger-ui page for your REST API should now be served at
`http://yourapp.com/docs`.

#### Example 2 (Serving static resource files)

If you already have a set of Swagger-compliant JSON files, you can configure this bundle to serve them for you in such a way that `swagger-ui` can consume it properly:

1. Place all JSON files inside a directory (doesn't have to be public).
2. Register two routes, one for the bare resource-list, and the other for the actual stylized documentation:

    ```yaml
     # app/config/routing.yml

    al_swagger_ui_static_resources:
        resource: @ALSwaggerUI/Resources/config/static_resources_routing.yml
        prefix: /swagger-docs

     al_swagger_ui:
        resource: @ALSwaggerUIBundle/Resources/config/routing.yml
        prefix: /docs
    ```

    This will result in a `/swagger-docs` route to return the bare resource-list, and `/swagger-docs/<resource_name>` to serve API declarations.  The `/docs` route will then allow SwaggerUI to consume them.

3. Configure the `static_resources` config:

    ```yaml
    al_swagger_ui:
        static_resources:
            resource_dir: app/Resources/swagger-docs
            resource_list_filename:  api-docs.json
        resource_list: al_swagger_ui_static_resource_list
    ```

Setting `resource_list` to `al_swagger_ui_static_resource_list` will then point `swagger-ui` to the right direction.
