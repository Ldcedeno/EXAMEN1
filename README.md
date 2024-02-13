// See https://aka.ms/new-console-template for more information

int num = 15;
int numActual = 0;

int[] numeroFactura = new int[num];
int[] numeroPlaca = new int[num];
int[] tipoVehiculo = new int[num];
int[] numeroCaseta = new int[num];
int[] montoPagar = new int[num];
int[] montoPagaCliente = new int[num];
int[] vuelto = new int[num];
string[] fecha = new string[num];
string[] hora = new string[num];

int opcion = 0;
string consulta = "";

menu();

void menu()
{
    do
    {
        mostrarMenu();
        opcion = int.Parse(Console.ReadLine());

        switch (opcion)
        {
            case 1: inicializar(); break;
            case 2: ingresarPasoVehicular(); break;
            case 3: consultarPorNumeroDePlaca(); break;
            case 4: consultarPorNumeroDePlaca(); break;
            case 5: reporteDatos(); break;
        }
    } while (opcion != 6);
}

void mostrarMenu()
{
    Console.WriteLine();
    Console.WriteLine("**********MENU***********");

    Console.WriteLine("1-Inicializar Vectores");
    Console.WriteLine("2-Ingresar paso vehicular");
    Console.WriteLine("3-Consulta de vehículos x Número de Placa");
    Console.WriteLine("4-Modificar Datos Vehículos x número de Placa");
    Console.WriteLine("5-Reporte Todos los Datos de los vectores");
    Console.WriteLine("6-Salir");

    Console.WriteLine("Digite una opcion...");
}

void inicializar()
{
    numActual = 0;

    numeroFactura = Enumerable.Repeat(0, num).ToArray<int>();
    numeroPlaca = Enumerable.Repeat(0, num).ToArray<int>();
    tipoVehiculo = Enumerable.Repeat(0, num).ToArray<int>();
    numeroCaseta = Enumerable.Repeat(0, num).ToArray<int>();
    montoPagar = Enumerable.Repeat(0, num).ToArray<int>();
    montoPagaCliente = Enumerable.Repeat(0, num).ToArray<int>();
    vuelto = Enumerable.Repeat(0, num).ToArray<int>();
    fecha = Enumerable.Repeat("", num).ToArray<string>();
    hora = Enumerable.Repeat("", num).ToArray<string>();

    Console.WriteLine();
    Console.WriteLine("Los arreglos han sido inicializados");
}

void ingresarPasoVehicular()
{
    Console.Clear();
    string continuar = "S";
    do
    {
        Console.WriteLine("Numero Factura: ");
        numeroFactura[numActual] = int.Parse(Console.ReadLine());

        Console.WriteLine("Numero Placa: ");
        numeroPlaca[numActual] = int.Parse(Console.ReadLine());

        Console.WriteLine("Fecha: ");
        fecha[numActual] = Console.ReadLine();

        Console.WriteLine("Hora: ");
        hora[numActual] = Console.ReadLine();

        Console.WriteLine("Tipo de Vehiculo [1=Moto    2=Vehículo Liviano   3=Camión o Pesado   4=Autobús]: ");
        string dato = Console.ReadLine();
        tipoVehiculo[numActual] = verificarTipoDeVehiculo(dato);

        Console.WriteLine("Numero de caseta [1=caseta 1    2=caseta 2  3=caseta 3]: ");
        dato = Console.ReadLine();
        numeroCaseta[numActual] = verificarNumeroDeCaseta(dato);

        calcularMontoAPagar(numActual);
        Console.WriteLine($"Monto Pagar: {montoPagar[numActual]}");

        Console.WriteLine("Paga con: ");
        int pagaCon = int.Parse(Console.ReadLine());
        verificarPagaCliente(pagaCon, numActual);

        calcularVuelto(numActual);
        Console.WriteLine($"Vuelto: {vuelto[numActual]}");

        numActual++;

        Console.WriteLine();
        Console.WriteLine("Desea continuar? [S/N]: ");
        continuar = Console.ReadLine();

    } while (continuar.ToUpper() == "S" || numActual == num);
}

int verificarTipoDeVehiculo(string nuevoDato)
{
    int tipoDeVehiculoActual = int.Parse(nuevoDato);
    while (0 >= tipoDeVehiculoActual || tipoDeVehiculoActual >= 5)
    {
        Console.WriteLine("El tipo de vehiculo debe ser [1=Moto    2=Vehículo Liviano   3=Camión o Pesado   4=Autobús]: ");
        tipoDeVehiculoActual = int.Parse(Console.ReadLine());
    }
    return tipoDeVehiculoActual;
}

int verificarNumeroDeCaseta(string nuevoDato)
{
    int numeroDeCasetaActual = int.Parse(nuevoDato);
    while (0 >= numeroDeCasetaActual || numeroDeCasetaActual >= 4)
    {
        Console.WriteLine("El tipo  de caseta [1=caseta 1    2=caseta 2  3=caseta 3]: ");
        numeroDeCasetaActual = int.Parse(Console.ReadLine());
    }
    return numeroDeCasetaActual;
}

void calcularMontoAPagar(int indice)
{
    switch (tipoVehiculo[indice])
    {
        case 1:
            {
                montoPagar[indice] = 500;
            }; break;
        case 2:
            {
                montoPagar[indice] = 700;
            }; break;
        case 3:
            {
                montoPagar[indice] = 2700;
            }; break;
        case 4:
            {
                montoPagar[indice] = 3700;
            }; break;
    }
}

void verificarPagaCliente(int pagaCon, int indice)
{
    while (pagaCon < montoPagar[indice])
    {
        Console.WriteLine("Por favor ingrese un monto mayor o igual al monto a pagar: ");
        pagaCon = int.Parse(Console.ReadLine());
    };
    montoPagaCliente[indice] = pagaCon;
}

void calcularVuelto(int indice)
{
    vuelto[indice] = montoPagaCliente[indice] - montoPagar[indice];
}

void consultarPorNumeroDePlaca()
{
    bool encontrado = false;
    Console.WriteLine();
    Console.WriteLine("Digite el numero de PLACA a consultar");
    consulta = Console.ReadLine();
    for (int i = 0; i < num; i++)
    {
        if (int.Parse(consulta) == numeroPlaca[i])
        {
            if (opcion == 4)
            {
                modificarDatos(i);
            }
            else
            {
                imprimirDatos(i);
            }
            encontrado = true;
        }
    }

    if (encontrado == false)
    {
        Console.WriteLine("Pago no se encuentra registrado");
    }
}

void imprimirDatos(int indice)
{
    Console.Clear();
    Console.WriteLine($"Numero de factura: {numeroFactura[indice]}");
    Console.WriteLine($"Numero de placa: {numeroPlaca[indice]}");
    Console.WriteLine($"Fecha: {fecha[indice]}");
    Console.WriteLine($"Hora: {hora[indice]}");
    Console.WriteLine($"Tipo de Vehiculo [1=Moto    2=Vehículo Liviano   3=Camión o Pesado   4=Autobús]: {tipoVehiculo[indice]}");
    Console.WriteLine($"Numero de caseta [1=caseta 1    2=caseta 2  3=caseta 3]: {numeroCaseta[indice]}");
    Console.WriteLine($"Monto Pagar: {montoPagar[indice]}");
    Console.WriteLine($"Paga con: {montoPagaCliente[indice]}");
    Console.WriteLine($"Vuelto: {vuelto[indice]}");
}

void modificarDatos(int indice)
{
    Console.Clear();
    Console.WriteLine($"A-Numero de factura: {numeroFactura[indice]}");
    Console.WriteLine($"B-Numero de placa: {numeroPlaca[indice]}");
    Console.WriteLine($"C-Fecha: {fecha[indice]}");
    Console.WriteLine($"D-Hora: {hora[indice]}");
    Console.WriteLine($"E-Tipo de Vehiculo [1=Moto    2=Vehículo Liviano   3=Camión o Pesado   4=Autobús]: {tipoVehiculo[indice]}");
    Console.WriteLine($"F-Numero de caseta [1=caseta 1    2=caseta 2  3=caseta 3]: {numeroCaseta[indice]}");
    Console.WriteLine($"Monto Pagar: {montoPagar[indice]}");
    Console.WriteLine($"G-Paga con: {montoPagaCliente[indice]}");
    Console.WriteLine($"Vuelto: {vuelto[indice]}");

    Console.WriteLine("Seleccione opcion a modificar");
    string modifica = Console.ReadLine();

    Console.WriteLine("Nuevo Dato: ");
    string nuevoDato = Console.ReadLine();

    switch (modifica.ToUpper())
    {
        case "A":
            {
                numeroFactura[indice] = int.Parse(nuevoDato);
            }; break;
        case "B":
            {
                numeroPlaca[indice] = int.Parse(nuevoDato);
            }; break;
        case "C":
            {
                fecha[indice] = nuevoDato;
            }; break;
        case "D":
            {
                hora[indice] = nuevoDato;
            }; break;
        case "E":
            {
                calcularMontoAPagar(indice);
                tipoVehiculo[indice] = int.Parse(nuevoDato);
                calcularVuelto(indice);
            }; break;
        case "F":
            {
                numeroCaseta[indice] = int.Parse(nuevoDato);
            }; break;
        case "G":
            {
                verificarPagaCliente(int.Parse(nuevoDato), indice);
                calcularVuelto(indice);
            }; break;
    }

    imprimirDatos(indice);
}

void reporteDatos()
{
    Console.Clear();
    Console.WriteLine("**********REPORTE DE VEHICULOS***********");
    Console.WriteLine("N Factura    Placa   Tipo vehiculo   Caseta  Monto Pagar  Paga con    Vuelto");
    Console.WriteLine("-------------------------------------------------------------------------------");

    int totalMonto = 0;
    for (int i = 0; i < numActual; i++)
    {
        Console.WriteLine($"{numeroFactura[i]}  {numeroPlaca[i]}  {tipoVehiculo[i]}  {numeroCaseta[i]}  {montoPagar[i]}  {montoPagaCliente[i]}  {vuelto[i]}");
        totalMonto += montoPagar[i];
    }

    Console.WriteLine("-------------------------------------------------------------------------------");
    Console.WriteLine($"Cantidad de vehiculos: {numActual}");
    Console.WriteLine($"Total: {totalMonto}");
}
