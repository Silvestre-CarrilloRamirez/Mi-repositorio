/******************************************************************************

Codigo que genera el RFC de una persona
Autor: Silvestre Carrillo Ramirez
Fecha: 10/08/2025

*******************************************************************************/
#include <iostream>
#include <vector>
#include <string>

//Diccionario de palabras no permitidas
const std::vector<std::string> palabrasProhibidas = {
    "PENE", "POPO", "PUTO", "CACA", "PITO" 
};

//Verificar y modificar palabras del Diccionario
std::string corregirRFC(const std::string& rfc) {
    for (const std::string& palabra : palabrasProhibidas) {
        if (rfc == palabra) {
            return rfc.substr(0, 3) + "X"; // Remplaza la última letra por 'X'
        }
    }
    return rfc;
}

// Función para obtener la primera vocal interna de una cadena
char obtenerPrimeraVocalInterna(const std::string& str) {
    for (size_t i = 1; i < str.length(); ++i) {
        char c = toupper(str[i]);
        if (c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U')
        return c;
    }
    return 'X'; //Si no encuentra ninguna vocal interna, se usa una X
}

//Verifica si el año es bisieto
bool esBisiesto(int anio) {
    return (anio % 4 == 0 && anio % 100 != 0) || (anio % 400 == 0);
}

bool fechaValida(const std::string& fecha) {
    if (fecha.size() !=10 || fecha [4] != '-' || fecha [7] != '-')
        return false;
    
    int anio = std::stoi(fecha.substr(0, 4));
    int mes  = std::stoi(fecha.substr(5, 2));
    int dia  = std::stoi(fecha.substr(8, 2));
    
    if (mes < 1 || mes > 12) return false;
    
    int diasPorMes[] = {31, (esBisiesto(anio) ? 29 : 28), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    if (dia < 1 || dia > diasPorMes [mes - 1]) return false;
    
    return true;
}


// Función principal para calcular el rfc
std::string calcularRFC(const std::string& nombre, 
                        const std::string& apellidoPaterno, 
                        const std::string& apellidoMaterno, 
                        const std::string& fechaNacimiento) {
    
    // Se declara la variable en donde guardaremos el rfc
    std::string rfc;
    
    // Se obtiene la letra inicial y la primera vocal interna del aperllioPaterno
    char letraInicial = toupper(apellidoPaterno[0]);
    char VocalInterna = obtenerPrimeraVocalInterna(apellidoPaterno);
    
    // Se obtiene la inicial del apellido materno o se usa una'X' si no existe
    char inicialApellidoMaterno = (!apellidoMaterno.empty()) ? toupper(apellidoMaterno[0]) : 'X';
    
    // Se obtiene la inicial del primer nombre o se usa una 'X' si no existe
    char inicialNombre = toupper(nombre[0]);
    
    // Se obtiene los dos últimos digitos del año de fechaNacimiento
    std::string anio = fechaNacimiento.substr(2, 2);
    
    // Se obtiene el mes y el día de fechaNacimiento
    std::string mes = fechaNacimiento.substr(5, 2);
    std::string dia = fechaNacimiento.substr(8, 2);
    
    // Se contruye el RFC sumando el texto de la información que se proporcionó
    rfc = letraInicial;
    rfc += VocalInterna;
    rfc += inicialApellidoMaterno;
    rfc += inicialNombre;
    
    // Aqui se compara con la BD la palabra mal formada y se sustituye con la letra 'X'
    rfc = corregirRFC(rfc);
    
    // Si hay corrección se continuá generando el rfc
    rfc += anio;
    rfc += mes;
    rfc += dia;
    return rfc;
}

int main() {
    // Envia a consola los datos que requiere para solicitar nombre, apellido y fechaNacimiento
    std::string nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento;
    std::cout << "Introduce tu nombre: ";
    std::getline(std::cin, nombre);
    std::cout << "Introduce tu apellido paterno: ";
    std::getline(std::cin, apellidoPaterno);
    std::cout << "Introduce tu apellido materno (Si no cuenta con apellido materno, presiona Enter): ";
    std::getline(std::cin, apellidoMaterno);
    
    do {
        std::cout << "Introduce la fecha de nacimiento en formato (YYYY-MM-DD): ";
        std:getline(std::cin, fechaNacimiento);
        
        if (!fechaValida(fechaNacimiento)) {
            std::cout <<"Fecha inválida. Intenta de nuevo.\n";
        } else {
            break;
        }
    } while (true);
    
    std::string rfc = calcularRFC(nombre, apellidoPaterno, apellidoMaterno, fechaNacimiento);
    std::cout << "RFC generado: " << rfc << std::endl;
    
    return 0;
}


