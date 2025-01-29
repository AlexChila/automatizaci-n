Paso 1: Digitalización
python
CopiarEditar
from PIL import Image
import pytesseract

# Extraer texto desde imágenes o PDFs
image = Image.open('documento_escaneado.jpg')
texto_extraido = pytesseract.image_to_string(image, lang='spa')
print(texto_extraido)
Paso 2: Limpieza y Estandarización
python
CopiarEditar
import pandas as pd

# Crear DataFrame con los datos extraídos
data = {'columna1': [texto_extraido]}  # Datos extraídos del OCR
df = pd.DataFrame(data)

# Limpieza de datos
df['columna1'] = df['columna1'].str.strip()  # Eliminar espacios
df.dropna(inplace=True)  # Eliminar valores nulos

print(df)
Paso 3: Conexión y Almacenamiento en SQL
python
CopiarEditar
from sqlalchemy import create_engine

# Crear conexión a la base de datos
engine = create_engine('postgresql://usuario:contraseña@localhost:5432/mi_basedatos')

# Insertar los datos en una tabla
df.to_sql('mi_tabla', engine, if_exists='append', index=False)

print("Datos almacenados en la base de datos.")
________________________________________
5. Análisis de Resultados
sql
CopiarEditar
SELECT * FROM mi_tabla LIMIT 10;








5. PARA CARGA A BASE DE DATO CON PYTHON

import pandas as pd
from sqlalchemy import create_engine

# CONFIGURAR CONEXIÓN A LA BASE DE DATOS
def conectar_base_datos(usuario, contraseña, host, puerto, nombre_bd):
    try:
        # Crear URL de conexión
        url_conexion = f"postgresql://{usuario}:{contraseña}@{host}:{puerto}/{nombre_bd}"
        engine = create_engine(url_conexion)
        print("Conexión exitosa a la base de datos.")
        return engine
    except Exception as e:
        print("Error al conectar a la base de datos:", e)
        return None

# CARGAR DATOS EN UNA TABLA
def cargar_datos(engine, tabla, archivo_csv):
    try:
        # Leer archivo CSV
        df = pd.read_csv(archivo_csv)

        # Limpiar y preprocesar datos (ejemplo: eliminar duplicados y NaN)
        df = df.drop_duplicates().dropna()

        # Cargar datos en SQL
        df.to_sql(tabla, engine, if_exists='append', index=False)
        print(f"Datos cargados exitosamente en la tabla '{tabla}'.")https://github.com/AlexChila/automatizaci-n/blob/main/README.md
    except Exception as e:
        print("Error al cargar los datos:", e)

# EJECUCIÓN PRINCIPAL
if __name__ == "__main__":
    # Configuración de la base de datos
    usuario = "tu_usuario"
    contraseña = "tu_contraseña"
    host = "localhost"  # Cambiar si es un servidor remoto
    puerto = "5432"  # Por defecto para PostgreSQL
    nombre_bd = "nombre_de_tu_base_datos"
    archivo_csv = "ruta_al_archivo.csv"
    tabla_destino = "nombre_de_la_tabla"

    # Conectar a la base de datos
    engine = conectar_base_datos(usuario, contraseña, host, puerto, nombre_bd)

    if engine:
        # Cargar datos en la tabla
        cargar_datos(engine, tabla_destino, archivo_csv)
