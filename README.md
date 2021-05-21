# Remap-y-robot-rbx1
Se mostrará como es posible realizar la conexiòn remap con el robot rbx1 en ROS.
## 1. Creamos un espacio de trabajo
Abrimos un terminal y ejecutamos lo siguiente:
```
mkdir -p ~/catkin_ws/scr
```
Luego nos dirigimos a la carpeta del espacio de trabajo:
```
cd catkin_ws
```
Ahora compilamos el espacio de trabajo para poder crear los paquetes correspondientes:
```
catkin_make
```
## 2. Instalamos el repositorio RBX1
Para esto abriremos un términal y bastará con ejecutar la siguiente instrucción: 
```
sudo apt-get install ros-kinetic-rbx1
```
*Es evidente que estamos trabajando con ROS kinetic, se tendrá que utilizar la declaración adecuada según la distribución de ROS con la que estemos trabajando.

Es preciso señalar que existe una alternativa de instalación, pues es posible descargarlo directamente desde github.
Para esto nos dirigimos a la carpeta "src" que está dentro de nuestro espacio de trabajo:
```
cd ~/catkin_ws/src
```
Seguidamente ejecutamos la siguiente línea para proceder con la descarga:
```
git clone https://github.com/pirobot/rbx1.git
```
## 3. Ejecutamos el archivo .launch
Gracias a esto es posible ejecutar varios nodos y paquetes a la vez. 
Para esto es necesario inicializar primero el entorno ROS:
```
roscore
``` 
En este caso, trabajaremos con el archivo fake_turtlebot.launch, por lo que lo ejecutamos:
```
roslaunch rbx1_bringup fake_turtlebot.launch
```
En este punto, podemos abrir una interfaz gráfica para poder observar el robot, gracias a la función rviz. La cual nos abre entorno 3D, que facilita la apreciación de simulaciones y procesos. Para ejecutarlo, ejecutamos la siguiente instrucción:
```
rosrun rviz rviz -d`rospack find rbx1_nav`/sim.rviz
```  

## 4. Modificamos el archivo .launch
Para que el robot RBX1 pueda comunicarse con ROS, más precisamente con la función teleop_key, es necesario modificar un tema dentro del archivo fake_turtlebot.launch. 
Para esto ejecutamos la siguiente línea de código en un terminal nuevo:
```
rosrun turtlesim turtle_teleop_key
```
Ahora necesitamos observar los temas que están presentes, ejecutamos:
```
rostopic list
```
Se mostrará la lista de temas, entre ellos /cmd_vel y /turtle1/cmd_vel.
Para ubicar el tema que tenemos que modificar, matamos el proceso teleop_key. 
Al realizarlo, observaremos que el tema /turtle1/cmd_vel desaparece, por lo que este es el tema que estamos buscando.

Ubicado el tema, abrimos un nuevo terminal y nos dirigimos a la ruta que contiene el archivo .launch:
```
cd ~/catkin_ws/src/rbx1/rbx1_bringup/launch
```
Ahora editamos el archivo fake_turtlebot.launch, estando desde el terminal, ejecutamos:
```
gedit fake_turtlebot.launch
```
*También es posible ubicarlo desde el explorador de archivos y editarlo con la aplicación de su preferencia.
Ya estando en el archivo, buscamos la parte del còdigo que ejecuta el nodo _arbotix_ el cual está entre los símbolos "<->". En esa parte del código, antes de cerrar el nodo, agregamos la siguiente línea de código y luego guardamos los cambios:
```
<remap from="/cmd_vel" to="/turtle1/cmd_vel"/>
```
*Se adjunta el archivo ya modificado para trabajarlo sin problema.
Nos aseguramos de matar los procesos anteriormente ejecutados, luego repetimos el paso 3 y la instrucción del teleop_key.
De esta forma ahora se puede controlar el robot rbx1 en el entorno rviz mediante la función del teleop_key.   

## Cambio de velocidad del robot rbx1
Para esto es necesario matar el proceso teleop_key y luego ejecutamos la siguiente instrucción:

```
rosparam set teleop_turtle/scale_linear 0.5 && rosrun turtlesim turtle_teleop_key
```
## Visualización de nodos
La función rqt_graph hace posible una interfaz gráfica que nos permite apreciar de forma didáctica los nodos que se están ejecutando y la conexión entre estos, señalando incluso sus temas correspondientes. La instrucción es la siguiente:
```
rosrun rqt_graph rqt_graph
```

# Autores: 
- Miguel Flores Sierra  
- Jean Gonzales Leyva
- David Estacio Quiroz 

