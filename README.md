# **Práctica #5**

## **VPC**
*Amazon Virtual Private Cloud (Amazon VPC) es un servicio que permite lanzar recursos de AWS en una red virtual aislada de forma lógica que usted defina. Puede controlar todos los aspectos del entorno de red virtual, como la selección de su propio rango de direcciones IP, la creación de subredes y la configuración de tablas de enrutamiento y gateways de red. Puede utilizar tanto IPv4 como IPv6 para la mayoría de los recursos de la nube virtual privada, lo que ayuda a garantizar el acceso seguro y fácil a los recursos y las aplicaciones.*

*Como uno de los servicios esenciales de AWS, Amazon VPC facilita la personalización de la configuración de red de la VPC. Puede crear una subred con acceso público para los servidores web que tengan acceso a Internet. También permite colocar los sistemas backend, como los servidores de aplicaciones o las bases de datos, en una subred con acceso privado sin acceso a Internet. Con Amazon VPC, puede utilizar varias capas de seguridad, incluidos grupos de seguridad y listas de control de acceso a la red, para ayudar a controlar el acceso a las instancias de Amazon EC2 en cada subred.*

[Más información](https://docs.aws.amazon.com/es_es/vpc/latest/userguide/what-is-amazon-vpc.html)

|Nombre|IP|
|------|--|
|vpcpractica5_201020126| 10.0.0.0/16|

<br>
<br>

## **Internet Gateway**

*Una gateway de internet es un componente de la VPC de escalado horizontal, redundante y de alta disponibilidad que permite la comunicación entre su VPC e internet.*

*Un gateway de Internet sirve para dos fines: proporcionar un objetivo en sus tablas de ruteo de VPC para el tráfico direccionable de Internet y realizar la conversión de las direcciones de red (NAT) para las instancias que tengan asignadas direcciones IPv4 públicas. Para obtener más información, consulte Habilitar el acceso a Internet.*

*Un gateway de Internet admite el tráfico IPv4 e IPv6. No genera riesgos de disponibilidad ni restricciones del ancho de banda del tráfico de red. No hay ningún cargo adicional por tener una gateway de Internet en su cuenta.*


- Este *Internet Gateway* se asocia a la VPC y es por esto que la VPC tiene enlace a internet
- Esta relación se da de uno a uno, por lo que una VPC solo tiene un *Internet Gateway* y un *Internet Gateway* esta asociado a solo una VPC 

|Internet Gateway| VPC|
|-|-|
|ig_201020126| vpcpractica5_201020126|

<br>
<br>

## **Subnets**

*Tras crear la VPC, podrá añadir una o varias subredes en cada zona de disponibilidad. De forma opcional, puede añadir subredes en una zona local, que es una implementación de la infraestructura de AWS que acerca los servicios de informática, almacenamiento, base de datos y otros servicios selectos a los usuarios finales.*

[Más información](https://docs.aws.amazon.com/es_en/vpc/latest/userguide/VPC_Subnets.html)

|Nombre|IP|
|------|--|
|subredpublica_201020126|10.0.1.0/24|
|subredprivada_201020126|10.0.2.0/24|
<br>

- Cada subnet esta ligada a la VPC determinada
- A la subnet publica se le modifica el auto-assign IP settings, se activa la opción *Enable auto-assign public IPv4 address*

<br>
<br>

## **Routes**

*Las tablas de ruteo contienen conjuntos de reglas, denominadas rutas, que se usan para determinar adónde se dirige el tráfico de red desde su subred o gateway.*

*Su VPC tiene un enrutador implícito y utiliza las tablas de ruteo para controlar dónde se dirige el tráfico de red. Cada subred de la VPC debe estar asociada a una tabla de ruteo que controla el direccionamiento de la subred (tabla de ruteo de la subred). Puede asociar de forma explícita una subred con una tabla de ruteo particular. De lo contrario, la subred se asocia de forma implícita con la tabla de ruteo principal. La subred solo puede asociarse a una tabla de ruteo a la vez. Sin embargo, puede asociar varias subredes a la misma tabla de ruteo de la subred.*

*Puede asociar de forma opcional una tabla de ruteo a una gateway de Internet o a una gateway privada virtual (tabla de ruteo de gateway). Esto le permite especificar reglas de direccionamiento para el tráfico que entra en su VPC a través de la gateway.*

- Acá configuramos las tablas de enrutamiento y se ligan a la VPC determinada.
- A cada tabla de enrutamiento se les asignan las IP con las cuales van a tener comunicación.

|Tabla de enrutamiento|VPC|
|---|-----|
|practica5_public|vpcpractica5_201020126|
|practica5_private|vpcpractica5_201020126|
<br>

- A la publica se le tiene que asignar una IP publica para tener acceso a internet y para ello generamos un *Internet Gateway*

<br>

### **Route tables**
> practica5_public

- A esta se le agrega en la opcion de *Edit Routes* el *Internet Gateway*

|Destino|Tag|
|-|-|
|10.0.0.0/16|local|
|0.0.0.0/0|*Internet Gateway*|

<br>

>  practica5_private

|Destino|Tag|
|-|-|
|10.0.0.0/16|local|

<br>
<br>

## **EC2**
*Amazon Elastic Compute Cloud (Amazon EC2) es un servicio web que proporciona capacidad informática en la nube segura y de tamaño modificable. Está diseñado para simplificar el uso de la informática en la nube a escala web para los desarrolladores. La sencilla interfaz de servicios web de Amazon EC2 permite obtener y configurar capacidad con una fricción mínima. Proporciona un control completo sobre los recursos informáticos y puede ejecutarse en el entorno informático acreditado de Amazon*

[Más información](https://aws.amazon.com/es/ec2/?ec2-whats-new.sort-by=item.additionalFields.postDateTime&ec2-whats-new.sort-order=desc)

- Al crearlas se les determina que estan asociadas a la red *vpcpractica5_201020126*
> Publicas
- Las EC2 publicas son:
    - publica1_201020126
    - publica2_201020126

- Configure Security Group: *practica5_public1*

![](/images/img2.png)

> Privada

- Configure Security Group: *practica5_private*

![](/images/img3.png)

Voy a VPC/Route table/practica5_public/Subnet Associations/Edit Subnet Associations  y asociar la subredpublica_201020126

## Load Balancer
*Elastic Load Balancing distribuye automáticamente el tráfico de aplicaciones entrante a través de varios destinos, tales como las instancias de Amazon EC2, los contenedores, las direcciones IP, las funciones Lambda y los dispositivos virtuales. Puede controlar la carga variable del tráfico de su aplicación en una única zona o en varias zonas de disponibilidad. Elastic Load Balancing ofrece cuatro tipos de balanceadores de carga que cuentan con el nivel necesario de alta disponibilidad, escalabilidad automática y seguridad para que sus aplicaciones sean tolerantes a errores.*

![](/images/img4.png)

Para esta práctica seleccionamos el "Classic Load Blancer"

*El balanceador de carga clásico proporciona balanceo de carga básico en varias instancias de Amazon EC2 y funciona tanto en el nivel de solicitud como en el nivel de conexión. El balanceador de carga clásico está diseñado para aplicaciones que se crearon dentro de la red EC2-Classic.*

[Más informacion](https://aws.amazon.com/es/elasticloadbalancing/?elb-whats-new.sort-by=item.additionalFields.postDateTime&elb-whats-new.sort-order=desc).


Configuración final: 
![](/images/img1.png)

**DNS name:** lbp5201020126-1635633943.us-east-2.elb.amazonaws.com

![](/images/img6.png)

**Load balancer: lbp5201020126**

- Description
 ![](/images/img7.png)

 - Instances
 ![](/images/img8.png)

