Sistema de gestión de información hidroclimatológica
====================================================

Instalación
-----------

1) **Actualizar paquetes de servidor/sistema operativo**



2) **Instalar framework DJANGO ver. 3.x**


3) **Instalar base de datos POSTGRESQL ver. 12**



4) **Sincronizar relojes**


   Los relojes del sistema operativo y la base de datos deben estar iguales y en la misma zona horaria. Esto es importante para la carga de datos desde archivos.

5) **Instalar git**

6) **Crear carpeta de proyecto**

   .. code-block:: bash

      mkdir ~/proyecto

7) **Descargar el código fuente**

   .. code-block:: bash

      cd ~/proyecto
      git clone ssh://fonag@A.B.C.D/home/fonag/repositorio/paramh2o.git


8) **Instalar entorno virtual**

   .. code-block:: bash

      sudo apt-get install virtualenv


         
9) **Instalar paquetes requeridos en el entorno virtual**

   .. code-block:: bash
   
      cd ~/proyecto
      virtualenv -p python3 venv
      source venv/bin/activate
      (venv) cd paramh2o
      (venv) pip install -r requirements.txt


10) **Ejecutar las migraciones Django**

   .. code-block:: bash
   
      (venv) python manage.py migrate
      

11) **Copiar funciones de la base de datos**

   Estas son las funciones y disparadores (triggers) necesarios para el sistema realice actividades tales como: consultas de datos, insercción de datos por archivo y ejecución automática de cálculos de reportería (generación de horarios, diarios y mensuales).

   .. code-block:: bash
   
      (venv) python manage.py runscript instalar_funciones_postgres
      
      
12) **Programar ejecución de cálculo automático de reportes faltantes**
   Esto script tiene como finalidad desencadenar el cálculo de reportes horario, diario y mensual en caso de que se haya generado un problema en el flujo normal de cálculo.
   

   .. code-block:: bash
   
      crontab -e
      
      

   
         5 0 * * * /home/user/proyecto/venv/bin/python /home/user/proyecto/paramh2o/manage.py runscript generar_horario_loop
         5 1 * * * /home/user/proyecto/venv/bin/python /home/user/proyecto/paramh2o/manage.py runscript generar_diario_loop
         5 2 * * * /home/user/proyecto/venv/bin/python /home/user/proyecto/paramh2o/manage.py runscript generar_mensual_loop
 
 

   
      sudo service cron restart
      
