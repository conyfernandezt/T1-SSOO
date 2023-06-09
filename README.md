# T1-SSOO

### Consideraciones importantes

- Conectamos dos switch en la RED UC, porque de otra manera el servidor no podía reconocer las requests que le llegaban al siding.
- Si las requests no pueden regresar de alguno de los servidores de la RED UC (canvas o siding), simplemente se debe pingear al
Router Gateway desde dicho servidor. Esto se realiza desde la terminal con el siguiente comando: `ping 130.0.0.1`.
- Si de casualidad el HomeRouter de la RED Ruz capta a alguno de los dispositivos de la RED Casa Lesly, se puede solucionar simplemente
apagando y prendiendo el dispositivo para que se desconecten de la RED Ruz y se conecten a su WIFI de casa.

## Parte 4.1

1. ¿Cuál es el largo en bits de la dirección IP de destino?

   El largo es de 16 bits

2. ¿Cuál es la dirección IP de origen cuando el paquete se encuentra en el router central y el último dispositivo visitado es el router gateway de la red Casa Lesly?

   El IP de origen en este casa es la router de la casa de Lesly 140.100.2.2

3. ¿Cuál es la dirección IP de origen cuando el paquete se encuentra en el router central y el último dispositivo visitado es el router gateway de la red DNS?

   El IP de la dirección de origen en este caso de 9.9.0.2, la cual corresponde al DNS

4. Describa, en orden y separado por capas de entrada y salida, todo lo que ocurre con el paquete cuando este se encuentra en el servidor de la red DNS y el último dispositivo visitado es el router gateway de la red DNS.

* Capa de entrada:
    * Layer 1: Por fastethernet0/0
    * Layer 2: Ethernet II Header 0000.5853.4C97 >> 0001.64828692
    * Layer 3:  IP header src. IP: 9.9.0.2, Dest: 140.100.2.2

* Capa de salida: 
    * Layer 1: Port(s) serial2/0
    * Layer 2: HOLC frame HDLC
    * Layer 3: IP header src. IP: 9.9.0.2, Dest: 140.100.2.2

## Parte 4.2

1. ¿Cuál es el largo en bytes del HTTP Request del paquete HTTP?

   32 bytes
 
2. Describa que tipos de paquetes se están usando, es decir, decir que tipo de paquete son, por qué se usan estos paquetes y que deben contener.

* DNS: unidad de datos utilizada en la resolución de nombres de dominio en Internet. Paquetes que hace la conexión con el DNS cuando se hace el primer request a algun dominio.
(https://www.cloudflare.com/es-es/learning/dns/what-is-a-dns-server/)
* TCP: este tipo de paquete utilizadas para la comunicación confiable y orientada a la conexión entre dos dispositivos en una red, es decir que se está encargado de verificar que la conexión esté correctamente establecida. Los paquetes TCP son usados para para segmentar y enviar datos de manera eficiente y confiable a través de una red. Caracteristicas  importantes:
    * Conexión orientada: proceso de establecimiento de conexión de tres vías (handshake) que permite a ambos dispositivos acordar parámetros de comunicación.
    * Control de flujo: utiliza un mecanismo de control de flujo para regular la cantidad de datos que se envían entre los dispositivos
    * Segmentación y reensamblado: Los datos enviados se segmentan en más pequeños para facilitar la transmisión y el enrutamiento. Estos segmentos se vuelven a ensamblar en el destino para reconstruir los datos originales.
(https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:the-internet/xcae6f4a7ff015e7d:transporting-packets/a/transmission-control-protocol--tcp)
* HTTP: este tipo de paquete se utilizan para la comunicación entre un cliente y un servidor a través del protocolo HTTP. Se usa para el intercambio de información, por ejemplo solicitudes y respuestas, entre clientes y servidores web. Estos paquetes constan de una solicitud o una respuesta HTTP:
    * Solicitud HTTP: incluyen información como URL, los encabezados, entre otros
    * Respuesta HTTP: incluyen información como el código de estado HTTP (solicitud existosa o no), cuerpo de la respuesta, entre otros

3. Describa de forma ordenada que rutas toman los paquetes descritos en la pregunta anterior (especificar por donde pasan y en que orden)

   En primer lugar, si tiene que el paquete DNS sale del cliente y se dirige hacia el servidor DNS, para luego ser devuelto al cliente. Una vez completo este paso, el cliente manda un paquete de tipo TCP al servidor de destino (en este caso canvas o siding), con el objetivo de confirmar que las conexiones estén bien establecidas. Al igual que antes, una vez que llega al servidor, el paquete se devuelve al cliente. Una vez que llega al cliente, se comienza a enviar el paquete HTTP (que lleva el request) al servidor de destino antes visitado por el paquete TCP, este siempre antecedido por un paquete TCP para volver a confirmar que las conexiones estén correctamente establecidas. Una vez que el HTTP request llega al servidor, este le devuelve el paquete HTTP (response) con la información necesaria para el cleinte, como el código de estado y el cuerpo de la response.
   Por ejemplo, en el caso de que se genere un paquete de la casa de Lesly:
   1. Este va a salir de alguno de los dispositivos (PC, laptop o celular)
   2. Router de la casa
   3. Router gateway de subred casa Lesly
   4. Router central
   5. Router gateway del DNS
   6. Server DNS (aca se comienza a devolver)
   7. Router gateway del DNS
   8. Router central
   9. Router gateway de subred casa Lesly
   10. Dispositivo que mandó el package
   11. Router gateway de subred casa Lesly (aca comienza el envio del package TCP)
   12. Router central
   13. Router gateway de subred Canvas Siding
   14. Switch (dependiendo de a que server quiere llegar el switch toma)
   15. Server de Canvas o Siding (aca se comienza a devolver)
   16. Switch (dependiendo de a que server quiere llegar el switch toma)
   17. Router gateway de subred Canvas Siding
   18. Router central
   19. Router gateway de subred casa Lesly
   20. Dispositivo que mandó el package
   21. Router gateway de subred casa Lesly (aca comienza el envio del package HTTP)
   22. Router central
   23. Router gateway de subred Canvas Siding
   24. Switch (dependiendo de a que server quiere llegar el switch toma)
   25. Server de Canvas o Siding (aca se comienza a devolver con HTTP response)
   26. Switch (dependiendo de a que server quiere llegar el switch toma)
   27. Router gateway de subred Canvas Siding
   28. Router central
   29. Router gateway de subred casa Lesly
   30. Dispositivo que mandó el package

