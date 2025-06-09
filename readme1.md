Descripción del Notebook: Proceso ETL y EDA del Dataset Telecom X
Este notebook documenta las fases de Extracción, Transformación y Carga (ETL) y Análisis Exploratorio de Datos (EDA) sobre un dataset de clientes de Telecom X, con el objetivo de identificar factores que influyen en la tasa de evasión (Churn).

Contenido por Celda
Celda 1: Inicialización (Vacía)
# Celda 1 (ipython-input-0-d7e97302a5e7)
# Inicialización o celda vacía.
pass # No hay código para ejecutar
Usa el código con precaución
Celda 2: Inicio de la Fase de Extracción
# 📌 Extracción
Usa el código con precaución
Celda 3: Carga Inicial de Datos
# Celda 3
# Importación de librerías y carga inicial de datos desde JSON.
import pandas as pd
import requests

# URL del archivo JSON crudo
url = 'https://raw.githubusercontent.com/TAGARA-TECH/ChallengeTelecomX/main/TelecomX_Data.json'

# Obtener los datos desde la "API"
response = requests.get(url)
data_json = response.json()

# Convertir a DataFrame
df = pd.DataFrame(data_json)

# Mostrar las primeras filas
df.head()
Usa el código con precaución
Celda 4: Vista General del DataFrame
# Celda 4
# Vista general del DataFrame y tipos de datos.
print("Dimensiones del dataset:", df.shape)

# Columnas y tipos de datos
df.info()

# Tipos de datos por separado
print("\nTipos de datos:")
print(df.dtypes)

# Primeras filas para exploración visual
df.head()
Usa el código con precaución
Celda 5: Estadísticas y Valores Únicos
# Celda 5
# Estadísticas de variables numéricas y conteo de valores únicos por columna.
# Estadísticas de variables numéricas
df.describe()

# Ver valores únicos por columna (para identificar categóricas)
print("\nVer valores únicos por columna (para identificar categóricas):")
for col in df.columns:
    try:
        unique_vals = df[col].nunique()
        print(f"{col}: {unique_vals} valores únicos")
    except TypeError as e:
        print(f"⚠️ Error en columna '{col}': {e}")
        print(f"🔍 Primeros 2 valores no nulos en '{col}':")
        print(df[col].dropna().head(2).tolist())
Usa el código con precaución
Celda 6: Comentario sobre Columnas Anidadas
el dataset tiene columnas anidadas, es decir, varias columnas contienen diccionarios dentro de cada fila. Para poder analizarlas correctamente en pandas, hay que "aplanarlas" (flatten), es decir, convertir esas claves internas en columnas separadas.
Usa el código con precaución
Celda 7: Aplanamiento Manual (Primer Intento)
# Celda 7
# Expansión (aplanamiento) manual de las columnas anidadas: customer, phone, internet, account.
# Expandir la columna 'customer'
customer_df = df['customer'].apply(pd.Series)
df = df.drop('customer', axis=1).join(customer_df)

# Expandir la columna 'phone'
phone_df = df['phone'].apply(pd.Series)
df = df.drop('phone', axis=1).join(phone_df)

# Expandir la columna 'internet'
internet_df = df['internet'].apply(pd.Series)
df = df.drop('internet', axis=1).join(internet_df)

# Expandir la columna 'account'
account_df = df['account'].apply(pd.Series)

# Extraer también los valores anidados en 'Charges'
charges_df = account_df['Charges'].apply(pd.Series)
account_df = account_df.drop('Charges', axis=1).join(charges_df)

# Unir todo al DataFrame original
df = df.drop('account', axis=1).join(account_df)
Usa el código con precaución
Celda 8: Verificación Después del Aplanamiento
# Celda 8
# Verificación de la estructura y datos después del aplanamiento manual.
df.info()
df.head()
Usa el código con precaución
Celda 9: Resumen del Estado y Próximos Pasos
 Resumen del estado actual
🔹 Todas las columnas anidadas ya están desglosadas correctamente.

🔹 customer_id está completamente vacío (0 non-null), por lo tanto podés eliminarla.

🔹 La columna Total está como object, aunque debería ser numérica (float), probablemente porque hay algunos valores mal formateados (por ejemplo, strings vacíos, espacios o caracteres no numéricos).

Pasos a seguir:
Eliminar customer_id.

Convertir Total a numérico (y ver qué errores aparecen si los hay).

Verificar si hay valores nulos tras esa conversión.
Usa el código con precaución
Celda 10: Implementación de Pasos de Limpieza
# Celda 10
# Implementación de los primeros pasos de limpieza: eliminar customer_id y convertir Total a numérico.
# 1. Eliminar columna vacía
df.drop(columns=['customer_id'], inplace=True)

# 2. Convertir 'Total' a numérico, forzando errores a NaN si hay problemas
df['Total'] = pd.to_numeric(df['Total'], errors='coerce')

# 3. Verificar si hubo valores nulos generados en 'Total'
print(f"Valores nulos en 'Total': {df['Total'].isna().sum()}")
Usa el código con precaución
Celda 11: Estadísticas Descriptivas (Incluye 'all')
# Celda 11
# Estadísticas descriptivas incluyendo todas las columnas.
df.describe(include='all')
Usa el código con precaución
Celda 12: Distribución de Churn
# Celda 12
# Distribución de la variable objetivo 'Churn'.
df['Churn'].value_counts(normalize=True)
Usa el código con precaución
Celda 13: Conteo de Valores Nulos
# Celda 13
# Conteo de valores nulos en todas las columnas.
df.isna().sum()
Usa el código con precaución
Celda 14: Visualización de Distribución de Churn
# Celda 14
# Visualización de la distribución de Churn.
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(data=df, x='Churn')
plt.title("Distribución de Churn")
plt.show()
Usa el código con precaución
Celda 15: Celda Vacía
# Celda 15 (Vacía)
pass # No hay código
Usa el código con precaución
Celda 16: Verificación de Problemas en los Datos
## **Verificar si hay problemas en los datos que puedan afectar el análisis**
Esta etapa consiste en detectar y limpiar posibles problemas como:
Usa el código con precaución
Celda 17: Valores Ausentes
# Celda 17
# 1. Valores ausentes (NaN o None)
df.isna().sum().sort_values(ascending=False)
Usa el código con precaución
Celda 18: Porcentaje de Valores Faltantes
# Celda 18
# Porcentaje de valores faltantes.
(df.isna().sum() / len(df)).sort_values(ascending=False)
Usa el código con precaución
Celda 19: Valores Duplicados
# Celda 19
# 2. Valores duplicados
df.duplicated().sum()
Usa el código con precaución
Celda 20: Eliminación de Duplicados
# Celda 20
# Si hay duplicados, podés eliminarlos con:
df = df.drop_duplicates()
Usa el código con precaución
Celda 21: Verificación Post-Eliminación de Duplicados
# Celda 21
# Verificación después de eliminar duplicados.
print("Cantidad de filas duplicadas:", df.duplicated().sum())
Usa el código con precaución
Celda 22: Errores de Formato (Strip)
# Celda 22
# 3. Errores de formato (eliminar espacios en blanco).
for col in df.select_dtypes(include='object'):
    df[col] = df[col].str.strip()
#Si hay columnas con fechas (parece que no en este dataset), ahí se usaría .dt.normalize() como menciona el tip.
Usa el código con precaución
Celda 23: Inconsistencias en Categorías (Valores Únicos)
# Celda 23
# 4. Inconsistencias en categorías (listar valores únicos).
for col in df.select_dtypes(include='object'):
    print(f"{col}: {df[col].unique()}")
Usa el código con precaución
Celda 24: Tratamiento de Churn Vacío
# Celda 24
# Tratamiento de Churn: reemplazar '' con NaN.
# Churn: ['No' 'Yes' '']
df['Churn'] = df['Churn'].replace('', np.nan)
Usa el código con precaución
Celda 25: Tratamiento de MultipleLines Inconsistente
# Celda 25
# Tratamiento de MultipleLines: reemplazar 'No phone service' con 'No'.
# MultipleLines: ['No' 'Yes' 'No phone service']
df['MultipleLines'] = df['MultipleLines'].replace('No phone service', 'No')
Usa el código con precaución
Celda 26: Tratamiento de Servicios Internet Inconsistentes
# Celda 26
# Tratamiento de columnas con 'No internet service':
 cols_internet = ['OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
                 'TechSupport', 'StreamingTV', 'StreamingMovies']

for col in cols_internet:
    df[col] = df[col].replace('No internet service', 'No')
#Esta opción es muy común en análisis predictivo.
Usa el código con precaución
Celda 27: Comentario sobre Categorías Limpias
Todo lo demás:
'gender', 'Partner', 'Dependents', 'PhoneService', 'Contract', 'PaperlessBilling', 'PaymentMethod' → No tienen inconsistencias, ¡bien!
Usa el código con precaución
Celda 28-30: Celdas Vacías
# Celda 28, 29, 30 (Vacías)
pass # No hay código
Usa el código con precaución
Celda 31: Mini Script de Limpieza de Categorías
# Celda 31
# Mini script para consolidar algunas limpiezas de categorías.
import numpy as np

# Reemplazar valores vacíos en Churn
df['Churn'] = df['Churn'].replace('', np.nan)

# Reemplazar 'No phone service' en MultipleLines
df['MultipleLines'] = df['MultipleLines'].replace('No phone service', 'No')

# Reemplazar 'No internet service' en varias columnas
cols_internet = ['OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
                 'TechSupport', 'StreamingTV', 'StreamingMovies']
for col in cols_internet:
    df[col] = df[col].replace('No internet service', 'No')
Usa el código con precaución
Celda 32: Verificación Post-Mini Script
# Celda 32
# Verificación de categorías después del mini script de limpieza.
for col in df.select_dtypes(include='object'):
    print(f"{col}: {df[col].unique()}")
Usa el código con precaución
Celda 33-34: Celdas Vacías
# Celda 33, 34 (Vacías)
pass # No hay código
Usa el código con precaución
Celda 35: Inicio Sección Análisis Gemini
## Analisis de Gemini
Usa el código con precaución
Celda 36: Tratamiento de Churn Vacío (Sugerido por Gemini)
# Celda 36
# Verificación y tratamiento de Churn vacío, posiblemente sugerido por Gemini.
# Verificar cuántas filas tienen Churn vacío
print(df[df['Churn'] == ''].shape[0]) # Si ya se reemplazó '' con NaN, esto será 0.

# Opción 1: Eliminar filas con Churn vacío (si son pocas y no cruciales)
df = df[df['Churn'] != ''].copy() # Si ya se reemplazó '' con NaN, esta línea no eliminará nada con ''. Debería ser df.dropna(subset=['Churn']).copy()

# Opción 2: Reemplazar con NaN (comentada, ya hecha en celdas anteriores)
# df['Churn'] = df['Churn'].replace('', np.nan)
Usa el código con precaución
Celda 37: Inicio Fase de Transformación (Revisitada)
# 🔧 Transformación

Usa el código con precaución
Celda 38: Comentario sobre Desanidar Columnas (Revisitado)
## Fase de Transformación: Desanidando Columnas
El siguiente paso es crucial: vamos a desanidar las columnas que contienen diccionarios para que toda la información quede accesible y lista para el análisis. Como vimos, customer, phone, internet y account son las columnas que necesitamos expandir.
Usa el código con precaución
Celda 39: Desanidamiento Manual (Segundo Intento con Concat)
# Celda 39
# **REPETICIÓN DEL PROCESO DE DESANIDAMIENTO Y COMBINACIÓN**
# Carga datos originales de nuevo y realiza el aplanamiento usando pd.concat.
import pandas as pd
import requests

# Re-cargamos los datos para asegurarnos de tener el estado inicial (o el último que hayas guardado)
url = 'https://raw.githubusercontent.com/TAGARA-TECH/ChallengeTelecomX/main/TelecomX_Data.json'
response = requests.get(url)
data_json = response.json()
df = pd.DataFrame(data_json) # ¡Sobrescribe 'df' con datos crudos otra vez!

print("DataFrame original (primeras 3 filas):")
print(df.head(3))
print("\n---")

# --- Desanidando la columna 'customer' ---
df_customer_expanded = df['customer'].apply(pd.Series)
print("\nColumnas expandidas de 'customer' (primeras 3 filas):")
print(df_customer_expanded.head(3))
print("\n---")

# --- Desanidando la columna 'phone' ---
df_phone_expanded = df['phone'].apply(pd.Series)
print("\nColumnas expandidas de 'phone' (primeras 3 filas):")
print(df_phone_expanded.head(3))
print("\n---")

# --- Desanidando la columna 'internet' ---
df_internet_expanded = df['internet'].apply(pd.Series)
print("\nColumnas expandidas de 'internet' (primeras 3 filas):")
print(df_internet_expanded.head(3))
print("\n---")

# --- Desanidando la columna 'account' ---
df_account_expanded = df['account'].apply(pd.Series)
print("\nColumnas expandidas de 'account' (primeras 3 filas):")
print(df_account_expanded.head(3))
print("\n---")

# --- Combinando todo en un solo DataFrame ---
# 1. Eliminamos las columnas originales que contenían los diccionarios.
df = df.drop(columns=['customer', 'phone', 'internet', 'account'])

# 2. Concatenamos los DataFrames expandidos al DataFrame principal.
df = pd.concat([df, df_customer_expanded, df_phone_expanded, df_internet_expanded, df_account_expanded], axis=1)

print("\nDataFrame después de desanidar (primeras 5 filas):")
print(df.head())
print("\n---")

# Verificamos la nueva estructura y tipos de datos
print("\nDimensiones del DataFrame después de desanidar:", df.shape)
print("\nInformación del DataFrame después de desanidar:")
df.info()
Usa el código con precaución
Celda 40-42: Celdas Vacías
# Celda 40, 41, 42 (Vacías)
pass # No hay código
Usa el código con precaución
Celda 43: Verificación Post-Desanidamiento
# Celda 43
# Verificación del DataFrame después del segundo proceso de desanidamiento.
print(df.head())
print(df.info())
Usa el código con precaución
Celda 44: Siguientes Pasos de Transformación
### siguientes pasos:

Desanidar la columna Charges en Monthly y Total.
Convertir Monthly y Total a tipos numéricos y manejar los posibles valores no numéricos.
Tratar la columna Churn: reemplazar los valores vacíos con NA y eliminar las filas correspondientes.
Convertir las columnas categóricas al tipo category para optimizar el rendimiento y asegurar un manejo adecuado en el análisis.
Usa el código con precaución
Celda 45: Implementación Completa de Limpieza y Transformación
# Celda 45
# **Implementación más completa de la limpieza y transformación (Similar a la lógica de la función limpiar_dataset_telecom)**
# Carga datos originales de nuevo y realiza una limpieza y transformación integral.

import pandas as pd
import requests
import numpy as np # Necesario para pd.NA y fillna(0)

# Re-carga los datos para asegurarnos de tener el estado inicial (o el último que hayas guardado)
url = 'https://raw.githubusercontent.com/TAGARA-TECH/ChallengeTelecomX/main/TelecomX_Data.json'
response = requests.get(url)
data_json = response.json()
df = pd.DataFrame(data_json) # ¡Sobrescribe 'df' con datos crudos por tercera vez!

print("DataFrame original (primeras 3 filas):")
print(df.head(3))
print("\n---")

# --- Desanidando las columnas ---
df_customer_expanded = df['customer'].apply(pd.Series)
df_phone_expanded = df['phone'].apply(pd.Series)
df_internet_expanded = df['internet'].apply(pd.Series)
df_account_expanded = df['account'].apply(pd.Series)

# --- Desanidando la columna 'Charges' dentro de 'account' ---
# Verificamos si la columna 'Charges' existe en df_account_expanded antes de intentar desanidar
if 'Charges' in df_account_expanded.columns:
    df_charges_expanded = df_account_expanded['Charges'].apply(pd.Series)
    df_account_expanded = df_account_expanded.drop(columns=['Charges']) # Eliminar 'Charges' del df_account_expanded
    print("Columna 'Charges' desanidada correctamente.")
else:
    # Si 'Charges' no está en df_account_expanded, creamos un DataFrame vacío.
    df_charges_expanded = pd.DataFrame()
    print("Advertencia: La columna 'Charges' no se encontró en df_account_expanded para desanidar.")


# --- Combinando todo en un solo DataFrame ---
# 1. Eliminamos las columnas originales anidadas.
cols_to_drop_original = ['customer', 'phone', 'internet', 'account']
df = df.drop(columns=[col for col in cols_to_drop_original if col in df.columns]) # Eliminar solo si existen

# 2. Concatenamos los DataFrames expandidos, asegurando que no estén vacíos.
dataframes_to_concat = [df]
if not df_customer_expanded.empty: dataframes_to_concat.append(df_customer_expanded)
if not df_phone_expanded.empty: dataframes_to_concat.append(df_phone_expanded)
if not df_internet_expanded.empty: dataframes_to_concat.append(df_internet_expanded)
if not df_account_expanded.empty: dataframes_to_concat.append(df_account_expanded)
if not df_charges_expanded.empty: dataframes_to_concat.append(df_charges_expanded)

df = pd.concat(dataframes_to_concat, axis=1)


print("\nDataFrame después de desanidar (primeras 5 filas):")
print(df.head())
print("\n---")

# --- Eliminar la columna 'customerID' ya que no es necesaria para el análisis ---
if 'customerID' in df.columns:
    df.drop(columns=['customerID'], inplace=True)
    print("\nColumna 'customerID' eliminada.")
else:
    print("\nLa columna 'customerID' no se encontró (ya había sido eliminada o no se creó).")


# --- 2. Convertir 'Monthly' y 'Total' a tipos numéricos y manejar NaNs ---
# 'errors='coerce'' convertirá cualquier valor no numérico a NaN.
if 'Total' in df.columns:
    df['Total'] = pd.to_numeric(df['Total'], errors='coerce')
    df['Total'] = df['Total'].fillna(0) # Rellenar NaNs con 0
    print(f"Valores nulos en 'Total' después de rellenar con 0: {df['Total'].isna().sum()}")
else:
     print("La columna 'Total' no se encontró para convertir a numérico.")

if 'Monthly' in df.columns:
    df['Monthly'] = pd.to_numeric(df['Monthly'], errors='coerce')
    df['Monthly'] = df['Monthly'].fillna(0) # Rellenar NaNs con 0
    print(f"Valores nulos en 'Monthly' después de rellenar con 0: {df['Monthly'].isna().sum()}")
else:
    print("La columna 'Monthly' no se encontró para convertir a numérico.")


# --- 3. Tratamiento de valores en blanco en la columna 'Churn' ---
if 'Churn' in df.columns:
    print("Valores únicos en 'Churn' ANTES de la limpieza:", df['Churn'].unique())
    df['Churn'] = df['Churn'].replace('', pd.NA) # Reemplazamos '' por pd.NA
    df.dropna(subset=['Churn'], inplace=True) # Eliminamos las filas donde 'Churn' es NA
    print("Valores únicos en 'Churn' DESPUÉS de la limpieza:", df['Churn'].unique())
    print(f"Número de filas después de eliminar NAs en 'Churn': {len(df)}")
else:
    print("La columna 'Churn' no se encontró para limpieza.")

print("\n---")

# --- 4. Conversión de tipos de datos ---
# Lista de columnas categóricas.
categorical_cols = [
    'gender', 'Partner', 'Dependents', 'PhoneService', 'MultipleLines',
    'InternetService', 'OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
    'TechSupport', 'StreamingTV', 'StreamingMovies', 'Contract',
    'PaperlessBilling', 'PaymentMethod', 'Churn'
]

# Convertimos cada columna en la lista a tipo 'category'.
for col in categorical_cols:
    if col in df.columns: # Verificamos que la columna exista
        df[col] = df[col].astype('category')
    else:
        print(f"Advertencia: La columna '{col}' no se encontró para convertir a tipo 'category'.")

# Verificación final.
print("\nDimensiones finales del DataFrame después de todas las transformaciones:", df.shape)
print("\nInformación final del DataFrame:")
df.info()

if 'Churn' in df.columns:
    print("\nConteo de valores de Churn (final):")
    print(df['Churn'].value_counts())
Usa el código con precaución
Celda 46: Inicio Fase de EDA
### ¡Ahora pasamos a la fase de Análisis Exploratorio de Datos (EDA)!
El EDA es donde empezamos a descubrir patrones, relaciones y anomalías en los datos. Esto nos ayudará a entender qué factores podrían estar influyendo en el churn de los clientes de Telecom X.

Podemos abordar el EDA de varias maneras. Te sugiero que comencemos con los siguientes pasos:

Análisis de la distribución de la variable objetivo (Churn): Ver la proporción de clientes que se fueron (Yes) versus los que se quedaron (No). Esto es fundamental para entender el desequilibrio de clases, si lo hay.
Análisis univariado de características categóricas:
Contar la frecuencia de cada categoría.
Visualizar la distribución de cada categoría (por ejemplo, con gráficos de barras).
Ver cómo cada categoría se relaciona con la variable Churn (por ejemplo, ¿los clientes con cierto tipo de contrato tienen más probabilidades de irse?).
Análisis univariado de características numéricas:
Obtener estadísticas descriptivas (media, mediana, desviación estándar, etc.).
Visualizar la distribución (por ejemplo, con histogramas o boxplots).
Ver cómo estas variables se relacionan con Churn (por ejemplo, ¿los clientes con facturas mensuales más altas o más bajas tienen más probabilidades de irse?).
Usa el código con precaución
Celda 47: Análisis Exploratorio de Datos (EDA)
# Celda 47
# Inicio del Análisis Exploratorio de Datos (EDA).
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# NOTA IMPORTANTE: Asumimos que el DataFrame 'df' ya está cargado y completamente
# transformado en tu entorno, tal como lo dejó la Celda 45.

print("--- Análisis Exploratorio de Datos (EDA) ---")

# --- 1. Análisis de la variable objetivo: Churn ---
print("\n--- Distribución de la variable objetivo 'Churn' ---")
churn_counts = df['Churn'].value_counts()
print(churn_counts)
print(f"Porcentaje de Churn: {churn_counts['Yes'] / len(df) * 100:.2f}%")
print(f"Porcentaje de No-Churn: {churn_counts['No'] / len(df) * 100:.2f}%")

# Visualización de Churn
plt.figure(figsize=(6, 4))
sns.countplot(data=df, x='Churn', palette='viridis')
plt.title('Distribución de Clientes con Churn y Sin Churn')
plt.xlabel('Churn')
plt.ylabel('Número de Clientes')
plt.show()

# --- 2. Análisis de Características Categóricas seleccionadas vs. Churn ---
# Lista de columnas categóricas para analizar.
categorical_features_to_analyze = ['gender', 'Partner', 'Dependents', 'Contract']

for col in categorical_features_to_analyze:
    print(f"\n--- Análisis de la característica: '{col}' ---")

    # Frecuencia de cada categoría
    print(f"Frecuencia de categorías en '{col}':")
    print(df[col].value_counts())

    # Tasa de Churn por categoría
    churn_rate_by_category = df.groupby(col)['Churn'].value_counts(normalize=True).unstack()

    # Manejar el caso donde una categoría no tiene 'Yes' o 'No' para Churn
    if 'No' not in churn_rate_by_category.columns: churn_rate_by_category['No'] = 0
    if 'Yes' not in churn_rate_by_category.columns: churn_rate_by_category['Yes'] = 0

    churn_rate_by_category['Churn_Rate (%)'] = churn_rate_by_category['Yes'] * 100
    print(f"\nTasa de Churn por '{col}':")
    print(churn_rate_by_category[['Churn_Rate (%)']])

    # Visualización: Gráfico de barras de recuento con 'hue'
    plt.figure(figsize=(8, 5))
    sns.countplot(data=df, x=col, hue='Churn', palette='magma')
    plt.title(f'Distribución de Churn por {col}')
    plt.xlabel(col)
    plt.ylabel('Número de Clientes')
    plt.xticks(rotation=0)
    plt.legend(title='Churn')
    plt.show()
Usa el código con precaución
Celda 48: Verificación de Duplicados (Repetición)
# Celda 48
# Verificación de duplicados (Repetición).
print("Cantidad de filas duplicadas:", df.duplicated().sum())
Usa el código con precaución
Celda 49: Eliminación de Duplicados (Repetición)
# Celda 49
# Eliminación de duplicados (Repetición).
df.drop_duplicates(inplace=True)
Usa el código con precaución
Celda 50: Verificación Post-Eliminación de Duplicados (Repetición)
# Celda 50
# Verificación después de eliminar duplicados (Repetición).
print("Cantidad de filas duplicadas:", df.duplicated().sum())
Usa el código con precaución
Celda 51: Normalización de Categorías (Repetición)
# Celda 51
# Normalización de categorías tipo ‘No internet service’ o ‘No phone service’ (Repetición).

columns_replace_no_internet = [
    'OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
    'TechSupport', 'StreamingTV', 'StreamingMovies'
]
columns_replace_no_phone = ['MultipleLines']

# Reemplazo de 'No internet service' por 'No'
for col in columns_replace_no_internet:
    if col in df.columns:
        df[col] = df[col].replace({'No internet service': 'No'})

# Reemplazo de 'No phone service' por 'No'
for col in columns_replace_no_phone:
    if col in df.columns:
        df[col] = df[col].replace({'No phone service': 'No'})

print("\nCategorías normalizadas: 'No internet service' y 'No phone service' reemplazadas por 'No'.")
Usa el código con precaución
Celda 52: Normalización de Categorías sin Warnings
# Celda 52
# Reemplazo de 'No internet service' y 'No phone service' sin warnings (Versión mejorada de Celda 51).
columns_replace_no_internet = [
    'OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
    'TechSupport', 'StreamingTV', 'StreamingMovies'
]
columns_replace_no_phone = ['MultipleLines']

for col in columns_replace_no_internet:
    if col in df.columns:
        df[col] = df[col].astype('object').replace({'No internet service': 'No'})
        df[col] = df[col].astype('category')

for col in columns_replace_no_phone:
    if col in df.columns:
        df[col] = df[col].astype('object').replace({'No phone service': 'No'})
        df[col] = df[col].astype('category')

print("\nCategorías normalizadas sin warnings.")
Usa el código con precaución
Celda 53: Verificación de Categorías Normalizadas
# Celda 53
# Verificar que ya no hay valores antiguos en esas columnas.
for col in columns_replace_no_internet + columns_replace_no_phone:
    if col in df.columns:
        print(f"{col} - Categorías actuales: {df[col].cat.categories.tolist()}")
Usa el código con precaución
Celda 54: Normalización de Formatos en Churn
# Celda 54
# Normalización de formatos en Churn (mapear Yes/No a True/False).
df['Churn'] = df['Churn'].map({'Yes': True, 'No': False})
Usa el código con precaución
Celda 55: Guardado Intermedio del Dataset Limpio
# Celda 55
# Guardado intermedio del dataset limpio a CSV.
df.to_csv("TelecomX_limpio.csv", index=False)
Usa el código con precaución
Celda 56: Definición de la Función de Limpieza
# Celda 56
# Definición de la función `limpiar_dataset_telecom` que encapsula el proceso ETL completo.
def limpiar_dataset_telecom(df_original):
    import pandas as pd
    import numpy as np

    # Copiamos el DataFrame original para no modificar el original
    df = df_original.copy()

    # --- Desanidar columnas ---
    df_customer = df['customer'].apply(pd.Series)
    df_phone = df['phone'].apply(pd.Series)
    df_internet = df['internet'].apply(pd.Series)
    df_account = df['account'].apply(pd.Series)

    # Desanidar 'Charges' dentro de 'account' si existe
    if 'Charges' in df_account.columns:
        df_charges = df_account['Charges'].apply(pd.Series)
        df_account.drop(columns=['Charges'], inplace=True)
    else:
        df_charges = pd.DataFrame()

    # Eliminar columnas anidadas originales
    df.drop(columns=['customer', 'phone', 'internet', 'account'], inplace=True, errors='ignore')

    # Combinar todos los DataFrames desanidados
    dataframes_combinados = [df, df_customer, df_phone, df_internet, df_account, df_charges]
    df = pd.concat([d for d in dataframes_combinados if not d.empty], axis=1)

    # Eliminar columna customerID si existe
    df.drop(columns=['customerID'], inplace=True, errors='ignore')

    # Convertir 'Total' y 'Monthly' a numérico
    for col in ['Total', 'Monthly']:
        if col in df.columns:
            df[col] = pd.to_numeric(df[col], errors='coerce').fillna(0)

    # Limpieza de columna 'Churn'
    if 'Churn' in df.columns:
        df['Churn'] = df['Churn'].replace('', pd.NA)
        df.dropna(subset=['Churn'], inplace=True)

    # Columnas categóricas
    cat_cols = [
        'gender', 'Partner', 'Dependents', 'PhoneService', 'MultipleLines',
        'InternetService', 'OnlineSecurity', 'OnlineBackup', 'DeviceProtection',
        'TechSupport', 'StreamingTV', 'StreamingMovies', 'Contract',
        'PaperlessBilling', 'PaymentMethod', 'Churn'
    ]
    for col in cat_cols:
        if col in df.columns:
            df[col] = df[col].astype('category')

    # Normalizar 'No internet service' y 'No phone service'
    columnas_no_internet = ['OnlineSecurity', 'OnlineBackup', 'DeviceProtection', 'TechSupport', 'StreamingTV', 'StreamingMovies']
    columnas_no_phone = ['MultipleLines']

    for col in columnas_no_internet:
        if col in df.
