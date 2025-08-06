Procedimientos de desarrollo de software
========================================
1.- Generar la aplicación en VS 2010 y copiarla a VS 2019
	Generar la aplicación en VS2010
 	Copiar del directorio cd VS2010 (C:\Users\Mariano\Documents\Visual Studio 2010\Projects) al directorio de VS2019 (C:\Users\Mariano\source\repos)
 	Abrir la aplicación con VS 2019 (Abrir un proyecto o una solución)
  	En "Redestinar proyectos", elegir "Sin actualización" y "Sin actualización"
   
2.- Copiar de _Comun el fichero MMDVer.h al directorio de fuentes

3.- Entrar en Proyecto->Propiedades
	Todas las configuraciones
		Eventos de compilación
			Línea de comandos: BuildInc.exe
   
4.- Abrir app.rc
	En app.rc -> Botón derecho
		Agregar recurso
			Versión -> Nuevo
   
5.- Cerrar app.rc

6.- En app.rc -> Botón derecho
		Ver código; contestar que sí a cerrar el fichero y guardar los cambios

7.- Cambiar en app.rc:

  - al principio:
		#include "MMDVer.h"
	
	- apartado Version:
		FILEVERSION MMD_VERT
		PRODUCTVERSION MMD_VERT
		"CompanyName", "Universitat de València - Departament de Fisiologia"
		"FileDescription", <Descripción de la aplicación>
		"FileVersion", MMD_VERT_TX
		"LegalCopyright", "Copyright (C) 2017 - Mariano J. Muñoz Díaz"
		"ProductName", <Nombre del producto>
		"ProductVersion", MMD_VERT_TX		
8.- Compilar; poner en MMDVer.h la versión y subversión correspondiente (1.1)
9.- Entrar en Proyecto->Propiedades
	Todas las configuraciones
		Propiedades de configuración
			Directorios de VC++
				Directorios de archivos de inclusión
					- Añadir al final: $(ProjectDir)\..\..\_Comun

10.- Ajuste de tamaño y título de la ventana principal
11.- Botones:
	1- Salir
		Poner el contenido de Salir.h
	2- Procesar
	... otros 
12.- Copiar de _Comun al directorio de fuentes y Añadir existentes
	IniStream.cpp
	IniStream.h

12.- Copiar de _Comun al directorio de fuentes y Añadir existentes
	Param.cpp
	Param.h

13.- Incluir en Form1.h

	#include "Param.h"

14.- Añadir al principio de la clase Form1:

	public:
		String^ sDirIni;			//Directorio del .ini
		CParam^	cParam;				//Parámetros

15.- Añadir en el código del constructor de Form1 (sustituyendo <Nombre de aplicación> por el nombre de la aplicación):

			cParam = gcnew CParam;

			//Path aplicación:         
			array<String^>^ arguments = Environment::GetCommandLineArgs();
			
			if (arguments->Length > 1)	//Hay directorio de .ini
			{
				sDirIni =  arguments[1];
			}
			else
			{
				sDirIni = Environment::GetFolderPath(Environment::SpecialFolder::LocalApplicationData);
			}

			sDirIni += "\\<Nombre de aplicación>";

			Directory::CreateDirectory(sDirIni);
			sDirIni += "\\<Nombre de aplicación>.ini";

			cParam->LeeParam(sDirIni);


16.- Añadir en el código del destructor de Form1:

			if (/*Validar() == */true)
			{
				cParam->EscribeParam(sDirIni);
			}

			delete cParam;

17.-

20.- Control DJ

	Incluir "MIDICmd.h"


30.- Syslog (RFC 3164)
	Datos de Syslog (guardados en .ini)
		SyslogIPn: normalmente localhost; si es necesario, poner la IP para envío remoto. Si es 0, no hay syslog
		Facility:
			Local0: Programas de generación de pulsos
			Local1: Programas de análisis

	IPs de syslog:
		El broadcast no suele funcionar, por lo que es mejor poner las IPs de destino del syslog, de esta forma incluso pueden enrutarse.
		SyslogIP0 .. SyslogIP9: En el .ini se pueden poner hasta 10 IPs de syslog; leer los datos en el arranque, y enviar UDP a las direcciones indicadas.
		Adicionalmente, se puede escuchar en el puerto de syslog para recibir IPs que quieren que se les envíe syslog; estas IPs se pueden añadir a los destinos
		de syslog y comenzar a enviarles mensajes
