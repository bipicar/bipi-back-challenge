# Prueba de acceso a Bipi - backend
En Bipi tenemos distintos tipos de vehículos a los que un cliente se puede suscribir y cada uno tiene un precio diferente según sus características.

Ahora, debemos implementar un sistema de códigos promocionales y se te ha encargado a ti desarrollar esta funcionalidad en nodejs.

El resultado final será implementar un API con 3 endpoints y a ser posible sin usar base de datos:

## Vehículos
Por un lado, tendremos un endpoint POST `/vehicles` el cual recibirá un json como este que servirá para cargar los vehículos disponibles:
```
[
  {
    _id: "5f6223d34a5a8ff0dc639dfa",
    price: 359.99,
    priceOnOffer: 299.99,
    isOnOffer: false
  },
  {
    _id: "5ebd1d03f8f44c7c6b7d4391",
    price: 559.99,
    priceOnOffer: 499.99,
    isOnOffer: true
  }
]
```
La respuesta será:
- `202` sin contenido cuando todo vaya bien
- `400` cuando alguno de los campos falte o no tenga el formato adecuado, teniendo en cuenta que todos los campos serán obligatorios.

## Códigos promocionales
Tendremos otro endpoint POST `/promocodes` que de igual forma recibirá un json como este:
```
[
  {
    _id: "5f77173b76b9921886a8315a",
    code: "FIAT50",
    discount: 50,
    type: "PERCENTAGE",
    vehicles: [5f6223d34a5a8ff0dc639dfa]
  },
  {
    _id: "5f77173976b9921886a83159",
    code: "MERCEDES100",
    discount: 100,
    type: "CREDIT",
    vehicles: [5ebd1d03f8f44c7c6b7d4391]
  },
  {
    _id: "5f77173976b9921886a83159",
    code: "ALL10",
    discount: 10,
    type: "PERCENTAGE",
    vehicles: []
  }
]
```
La respuesta será:
- `202` sin contenido cuando todo vaya bien
- `400` cuando alguno de los campos falte o no tenga el formato adecuado, teniendo en cuenta que todos los campos serán obligatorios.

Tendremos 2 tipos de códigos promocionales:
- `PERCENTAGE`: se tomará el valor de `discount` como un porcentaje, si fuera 10, restaríamos al precio del vehículo un 10%.
- `CREDIT`: se tomará el valor de `discount` como un valor absoluto, si fuera 10, restaríamos 10€ al precio del vehículo.

La propiedad `vehicles` define a qué vehículos se puede aplicar cada código, será un array con los ids de los vehículos. ⚠️ En caso de no tener ninguno se podrá aplicar a todos.

## Calculador de precios
Tendremos un endpoint GET `/calculator/{{id}}?code={{code}}`, siendo `{{id}}` el id del vehículo y `{{code}}` la propiedad `code` de un código promocional. 

La respuesta será:
- `400` cuando alguno de los campos falte o no tenga el formato adecuado y cuando el código no sea aplicable al vehículo
- `404` cuando el vehículo no exista
- `200` con un json como el siguiente, teniendo en cuenta que **siempre deben llegar las 3 propiedades**:
```
{
  price: 456.99,
  discountByOffer: 45
  discountByCode: 34
}
```
⚠️ Ten en cuenta que en caso de estar en oferta, el descuento del código promocional se debe aplicar sobre el precio de oferta.

# ¿Qué vamos a medir?
- El cumplimiento de contrato de entrada y salida de los endpoints.
- La solución implementada para el cálculo de precios.
- La estructura del proyecto.
- La optimización de las consultas a la base de datos si la usas o la solución alternativa que elijas.
- La cobertura de tests del endpoint `/calculator`.
- La puesta a punto para producción.
- La integración contínua.
