# Práctica #5

## VPC
|Nombre|IP|
|------|--|
|vpcpractica5_201020126| 10.0.0.0/16|

## Internet Gateway
- Este *Internet Gateway* se asocia a la VPC y es por esto que la VPC tiene enlace a internet
- Esta relacion se da de uno a uno, por lo que una VPC solo tiene un *Internet Gateway* y un *Internet Gateway* esta asociado a solo una VPC 

|Internet Gateway| VPC|
|-|-|
|ig_201020126| vpcpractica5_201020126|

## Subnets


|Nombre|IP|
|------|--|
|subredpublica_201020126|10.0.1.0/24|
|subredprivada_201020126|10.0.2.0/24|
<br>
- Cada subnet esta ligada a la VPC determinada
- A la subnet publica se le modifica el auto-assign IP settings, se activa la opción *Enable auto-assign public IPv4 address*


## Routes
Acá configuramos las tablas de enrutamiento y se ligan a la VPC determinada.
- A cada tabala de enrutamiento se les asignan las IP con las cuales van a tener comunicación.

|VPC|Tabla|
|---|-----|
|vpcpractica5_201020126|practica5_public|
|vpcpractica5_201020126|practica5_private|
<br>

- A la publica se le tiene que asignar una IP publica para tener acceso a internet y para ello generamos un *Internet Gateway*

### Route tables
#### practica5_public
- A esta se le agrega en la opcion de *Edit Routes* el *Internet Gateway*

|Destino|Tag|
|-|-|
|20.0.0.0/16|local|
|0.0.0.0/0|*Internet Gateway*|

#### practica5_private
|Destino|Tag|
|-|-|
|20.0.0.0/16|local|

## EC2

### Publicas
- Al crearlas se les determina que estan asociadas a la red *vpcpractica5_201020126*
- Configure Security Group

|Name| Type | Protocol | Source| Description|
|-|-|-|-|-|
|practica5_public1|SSH| TCP| 22 | 0.0.0.0/0|Se puede acceder desde cualquier IP|
|practica5_public1| All ICMP-IPv4| ICMP| 0.0.0.0/0| Dede cualquier IP se puede hacer ping|

### Privada

- Configure Security Group

|Name| Type | Protocol | Source| Description|
|-|-|-|-|-|
|practica5_public1|SSH| TCP | 10.0.1.169/32|Esta es la IP de la EC2 publica1, porque solo esta tiene acceso |
|practica5_public1| All ICMP-IPv4| ICMP| 10.0.1.169/32| Dede cualquier IP se puede hacer ping|

Tengo que regresar a VPC/Route table/practica5_public/subnet associations/eddit subnet associations  y asociar la subredpublica_201020126 con IP 10.0.1.0/24

