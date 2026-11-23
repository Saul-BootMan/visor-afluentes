# Visor de afluentes

Página de una sola pieza, sin dependencias de build. Todo su contenido —marcado,
lógica y datos— va **cifrado** dentro del propio `index.html`; la página pide una
clave de acceso y lo descifra en el navegador.

## Cómo está cifrado

| Pieza | Elección |
|---|---|
| Cifrado | AES-256-GCM (Web Crypto) |
| Derivación de clave | PBKDF2-SHA256, 310.000 iteraciones, sal de 16 bytes |
| Vector de inicialización | 12 bytes aleatorios por compilación |
| Autenticación | el tag GCM viaja al final del criptograma |

El navegador deriva la clave con la contraseña que se escribe en la portada. Si no
coincide, el descifrado falla por el tag y la página no muestra nada: en el código
fuente solo hay base64.

Requiere un contexto seguro (`https://` o `localhost`), que es donde el navegador
habilita `crypto.subtle`.

## Alcance de la protección

La clave protege frente a una mirada casual, no frente a alguien decidido: el
archivo es público y quien lo descargue puede probar contraseñas sin límite y sin
dejar rastro. Las 310.000 iteraciones encarecen cada intento, pero no convierten
una contraseña corta en una larga. No publiques aquí nada cuya filtración importe.

## Compilar

```
node cifrar.mjs <clave> <datos.json> index.html
```

El compilador toma `app.html`, `app.js`, `estilo.css` y el JSON de datos, los
empaqueta, los cifra y escribe el `index.html` final. Las fuentes y el compilador
no viajan en este repositorio.
