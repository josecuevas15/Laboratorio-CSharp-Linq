# Laboratorio-CSharp-Linq
### Como primer paso, habrá que crear la clase Patient y la colección para almacenar dichos datos.

```csharp
public class Patient
{
    public int id;
    public string name;
    public string lastname;
    public string sex;
    public double temperature;
    public int heartRate;
    public string specialty;
    public int age;
    public Patient(int id, string name, string lastname, string sex, double temperature, int heartRate, string specialty, int age ) {
        this.id = id;
        this.name = name;
        this.lastname = lastname;
        this.sex = sex;
        this.temperature = temperature;
        this.heartRate = heartRate;
        this.specialty = specialty;
        this.age = age;
    }
}
```
```csharp
List<Patient> patients = new List<Patient>
{
    new Patient(1, "John", "Doe", "Male", 36.8, 80, "general medicine", 44),
    new Patient(2, "Jane", "Doe", "Female", 36.8, 70, "general medicine", 43),
    new Patient(3, "Junior", "Doe", "Male", 36.8, 90, "pediatrics", 8),
    new Patient(4, "Mary", "Wien", "Female", 36.8, 80, "general medicine", 20),
    new Patient(5, "Scarlett", "Somez", "Female", 36.8, 110, "general medicine", 30),
    new Patient(6, "Brian", "Kid", "Male", 39.8, 80, "pediatrics", 11)
};
```
## 1 - Extraer la lista de pacientes que sean de la especialidad pediatrics y que tengan menos de 10 años.

### Solucion:
```csharp
var pediatricsUnderTen = patients.Where(p => p.specialty=="pediatrics" && p.age < 10)
                                  .ToList();

foreach (var patient in pediatricsUnderTen)
{
    Console.WriteLine($"NAME: {patient.name}  LAST NAME: {patient.lastname}" +
        $"  AGE: {patient.age}  SPECIALTY: {patient.specialty}");
}
```

## 2 - Queremos activar el protocolo de urgencia si hay algún paciente con ritmo cardíaco mayor que 100 o temperatura mayor a 39.

### Solucion:

```csharp
var heart100OrTemp39 = patients.Where(p => p.heartRate > 100 || p.temperature > 39)
                                  .ToList();

foreach (var patient in heart100OrTemp39)
{
    Console.WriteLine($"NAME: {patient.name}  LAST NAME: {patient.lastname}" +
        $"  HEART RATE: {patient.heartRate}  TEMPERATURE: {patient.temperature}");
}
```

## 3 - No podemos atender a todos los pacientes hoy por lo que vamos a crear una nueva coleccion y reasignar a los pacientes de pediatrics a general medicine

### Solucion:

```csharp
var pediatrics = patients.Where(p => p.specialty == "pediatrics")
                        .ToList();

foreach (var patient in pediatrics)
{
    patient.specialty = "general medicine";

    Console.WriteLine($"NAME: {patient.name}  LAST NAME: {patient.lastname}" +
        $"  SPECIALTY: {patient.specialty}");
}
```

## 4 - Queremos conocer de una sola consulta el numero de pacientes que estan asignado a general medicine y a pediatrics.

### Solucion:

```csharp
var specialties = patients.GroupBy(p => p.specialty);

foreach (var specialty in specialties) {
    Console.WriteLine($"SPECIALTY: {specialty.Key}  PATIENTS: {specialty.Count()}");
}
```
## 5 - Devuelve una lista nueva que por cada paciente se indique si tiene prioridad o no. Se tendrá prioridad si el ritmo cardiaco es mayor 100 o la temperatura es mayor a 39.

### Solucion:

```csharp
var patientsPriority = patients.Select(p => new{
    NAME = p.name,
    LASTNAME = p.lastname,
    HEARTRATE = p.heartRate,
    TEMPERATURE = p.temperature,
    Priority = p.heartRate > 100 || p.temperature > 39
}).ToList();
foreach (var patient in patientsPriority)
{
    
    Console.WriteLine(patient);

}
```
## 6 - Queremos conocer de una sola consulta La edad media de pacientes asignados a pediatrics y general medicine.

### Solucion:

 ```csharp
var specialtiesAvgAge = patients.GroupBy(p => p.specialty).Select(s => new
{
    SPECIALTY = s.Key,
    AvgAge = s.Average(p => p.age)
});

foreach (var specialty in specialtiesAvgAge)
{
    Console.WriteLine(specialty);
}
```
