# Importante
##### Algunos Kali indican error caundo tienes instalada una versión obsoleta de PostgreSQL (versión 15) en tu sistema y sugiere que instales las últimas versiones de los paquetes del servidor y del cliente (postgresql-16 y postgresql-client-16, respectivamente) y luego realices la actualización.

Para resolver este problema, sigue estos pasos:

1. **Actualizar la lista de paquetes:**
    
    `sudo apt update`

2. **Eliminar las versiones obsoletas de PostgreSQL:**
    
    `sudo apt remove postgresql-15 postgresql-client-15`
    
3. **Instalar las últimas versiones:**
    
    `sudo apt install postgresql-16 postgresql-client-16`
    
4. **Realizar la actualización del clúster de PostgreSQL:**
    
    
    `sudo pg_upgradecluster 15 main`
    
    Este comando actualizará el clúster de PostgreSQL de la versión 15 a la versión 16. Asegúrate de que PostgreSQL no esté en ejecución durante este proceso.
    
5. **Reiniciar el servicio de PostgreSQL:**
    
    `sudo systemctl restart postgresql`
    
6. **Verificar la versión instalada:** Para asegurarte de que PostgreSQL se haya actualizado correctamente, verifica la versión instalada con el siguiente comando:
    
    `psql --version`

### Ahora realizamos una actualizacion mas completa de Kali

Para actualizar tu sistema Kali Linux y asegurarte de que esté completamente actualizado, sigue estos pasos en la terminal:

1. **Actualizar la lista de paquetes:**
    
    `sudo apt update`
    
2. **Actualizar los paquetes instalados:**
	`sudo apt full-upgrade`
	
    `sudo apt upgrade`
    
3. **Actualizar los paquetes del sistema:**
    
    `sudo apt dist-upgrade`
    
4. **Eliminar paquetes no necesarios:**
    
    `sudo apt autoremove`
    
5. **Limpiar la caché de paquetes:**
    
    `sudo apt clean`
    
6. **Actualizar las firmas de los repositorios y los paquetes:**
    
    `sudo apt update`