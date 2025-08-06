/******************************************************************************

Codigo que permite realizar las 4 operaciones básicas, suma, resta, 
multiplicación, y división

Autor: Silvestre Carrillo 
Fecha 05/08/2025
*******************************************************************************/
#include <iostream> 

int main()
{
    float numero1, numero2;
    std::cout << "Escribe el primer número: ";
    std::cin >> numero1;
    
    std::cout << "Escribe el segundo número: ";
    std::cin >> numero2;
    
    float suma = numero1 + numero2;
    float resta = numero1 - numero2;
    float multiplicacion = numero1 * numero2;
    float division = numero1 / numero2;
    
    std::cout << "la suma es: " << suma << std::endl;
    std::cout << "la resta es: " << resta << std::endl;
    std::cout << "la multiplicacion es: " << multiplicacion << std::endl;
    std::cout << "la division es: " << division << std::endl;
    
    return 0;
}
