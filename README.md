# ¿Qué tacho? — Clasificación de residuos

Aplicación web estática que utiliza la cámara del dispositivo o una imagen seleccionada para identificar residuos y recomendar el tacho correspondiente.

La clasificación se ejecuta en el navegador mediante un modelo TensorFlow Lite (`modelo.tflite`). La interfaz está pensada especialmente para dispositivos móviles.

## Funcionalidades

- Clasificación en tiempo real desde la cámara.
- Selección de una foto como alternativa a la cámara en vivo.
- Cambio entre cámara trasera y frontal cuando el dispositivo lo permite.
- Pausar y reanudar la clasificación en vivo.
- Enfoque automático continuo y enfoque tocando la pantalla, si el navegador lo admite.
- Vista de las tres predicciones principales con sus porcentajes.
- Suavizado de las últimas predicciones para reducir cambios bruscos.
- Aviso cuando la confianza del modelo es menor al 60%.

## Clases reconocidas

| Clase | Tacho recomendado | Ejemplos |
|---|---|---|
| `peligroso` | Rojo | Pilas, baterías, focos, medicamentos, aerosoles y jeringas |
| `biological` | Marrón | Restos de comida, cáscaras y hojas |
| `glass` | Verde | Botellas y frascos |
| `metal` | Amarillo | Latas, tapas y chatarra |
| `paper_cardboard` | Azul | Hojas, periódicos y cajas |
| `plastic` | Blanco | Botellas PET, envases y bolsas |
| `trash` | Negro | Papel higiénico, ropa, calzado y tecnopor sucio |

## Requisitos

- Un navegador moderno compatible con JavaScript, WebRTC y TensorFlow.js.
- Permiso para acceder a la cámara si se usa la clasificación en vivo.
- Servir los archivos mediante `localhost` o HTTPS. Abrir `index.html` directamente puede impedir el acceso a la cámara por las restricciones de seguridad del navegador.

## Ejecución local

Desde la carpeta del proyecto, inicia cualquier servidor HTTP estático. Por ejemplo, si tienes Python instalado:

```bash
python -m http.server 8000
```

Después abre `http://localhost:8000` en el navegador.

También puedes usar la extensión **Live Server** de Visual Studio Code u otro servidor estático equivalente.

## Uso

1. Abre la aplicación en el navegador.
2. Espera a que aparezca `Modelo listo`.
3. Pulsa **Abrir cámara** o selecciona **Usar una foto**.
4. Coloca un solo residuo dentro del recuadro.
5. Consulta la categoría, el porcentaje de confianza y el tacho recomendado.

## Estructura del proyecto

```text
.
├── index.html     # Interfaz, estilos y lógica de la aplicación
├── modelo.tflite  # Modelo de clasificación TensorFlow Lite
├── clases.json    # Orden de las clases de salida del modelo
└── README.md      # Documentación del proyecto
```

## Detalles técnicos

- Entrada del modelo: imagen RGB de `224 × 224` píxeles.
- Inferencia: TensorFlow.js con backend CPU y soporte TensorFlow Lite.
- Dependencias del navegador: se cargan desde jsDelivr.
- Intervalo de clasificación en cámara: aproximadamente 150 ms.
- Predicción mostrada: promedio de hasta seis predicciones recientes.
- Umbral de confianza: `0.60`.

## Consideraciones

El modelo y la aplicación no sustituyen las normas locales de separación de residuos. Cuando la confianza sea baja, mejora la iluminación, centra el objeto y evita colocar varios residuos a la vez en el recuadro.
