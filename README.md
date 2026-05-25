# -Evaluacion-N-2-DRY7122-
Luis Bahamondez y Gian Corazzelli
import requests

# ==============================
# CONFIGURACIÓN API MAPQUEST
# ==============================

API_KEY = "Fi6ak3qo646FAjs9MnA41zTe3Oyk5Hxt"

# ==============================
# FUNCIÓN PARA CONSULTAR RUTA
# ==============================

def calcular_ruta(origen, destino):

    url = "http://www.mapquestapi.com/directions/v2/route"

    parametros = {
        "key": API_KEY,
        "from": origen,
        "to": destino,
        "unit": "k"
    }

    respuesta = requests.get(url, params=parametros)

    datos = respuesta.json()

    # Validación de errores
    if datos["info"]["statuscode"] != 0:
        print("\nError al calcular la ruta.")
        return

    ruta = datos["route"]

    distancia = float(ruta["distance"])

    tiempo_segundos = int(ruta["time"])

    horas = tiempo_segundos // 3600
    minutos = (tiempo_segundos % 3600) // 60
    segundos = tiempo_segundos % 60

    print("\n========== RESULTADO DEL VIAJE ==========")

    print(f"Ciudad Origen : {origen}")
    print(f"Ciudad Destino: {destino}")

    print(f"\nDistancia: {distancia:.2f} km")

    print(f"Duración del viaje: "
          f"{horas:.2f} horas, "
          f"{minutos:.2f} minutos, "
          f"{segundos:.2f} segundos")

    print("\n========== NARRATIVA DEL VIAJE ==========")

    for paso in ruta["legs"][0]["maneuvers"]:
        print(f"- {paso['narrative']}")

# ==============================
# PROGRAMA PRINCIPAL
# ==============================

while True:

    print("\n===== CALCULADOR DE RUTAS MAPQUEST =====")

    origen = input("Ingrese Ciudad de Origen (o q para salir): ")

    if origen.lower() == "q":
        print("Programa finalizado.")
        break

    destino = input("Ingrese Ciudad de Destino (o q para salir): ")

    if destino.lower() == "q":
        print("Programa finalizado.")
        break

    calcular_ruta(origen, destino)
