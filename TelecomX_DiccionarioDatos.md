
# 📘 Diccionario de Datos - Telecom X

Este diccionario explica cada una de las variables del dataset utilizado en el análisis de churn para Telecom X.

| **Columna**             | **Tipo de Dato**        | **Descripción** |
|-------------------------|--------------------------|------------------|
| `customerID`            | Categórico (texto)        | Identificador único por cliente. No se usa para análisis, pero útil para trazabilidad. |
| `Churn`                 | Categórico binario        | Variable objetivo. Indica si el cliente se fue (Yes/No). |
| `gender`                | Categórico                | Género del cliente: 'Male' o 'Female'. |
| `SeniorCitizen`         | Binario (0/1)             | Si el cliente tiene 65 años o más (1) o no (0). |
| `Partner`               | Categórico binario        | Si el cliente tiene pareja ('Yes'/'No'). |
| `Dependents`            | Categórico binario        | Si tiene dependientes a cargo. |
| `tenure`                | Numérico (discreto)       | Meses que lleva como cliente. Puede reflejar lealtad. |
| `PhoneService`          | Categórico binario        | Si tiene servicio telefónico. |
| `MultipleLines`         | Categórico                | Si tiene más de una línea telefónica. |
| `InternetService`       | Categórico                | Tipo de servicio de Internet: DSL, Fiber optic, None. |
| `OnlineSecurity`        | Categórico                | Si tiene suscripción de seguridad en línea. |
| `OnlineBackup`          | Categórico                | Si tiene backup en línea. |
| `DeviceProtection`      | Categórico                | Protección de dispositivo. |
| `TechSupport`           | Categórico                | Soporte técnico. Puede influir en la satisfacción. |
| `StreamingTV`           | Categórico                | Servicio de TV por cable. |
| `StreamingMovies`       | Categórico                | Servicio de películas. |
| `Contract`              | Categórico                | Tipo de contrato: mensual, 1 año, 2 años. Es clave para prever churn. |
| `PaperlessBilling`      | Categórico binario        | Si recibe factura electrónica. |
| `PaymentMethod`         | Categórico                | Tipo de pago (banco, tarjeta, débito automático, etc.). |
| `Charges.Monthly`       | Numérico (continuo)       | Total mensual facturado. Refleja nivel de consumo. |
| `Charges.Total`         | Numérico (continuo)       | Total gastado a lo largo del tiempo. Requiere cuidado con valores nulos o anómalos si el `tenure` es 0. |
