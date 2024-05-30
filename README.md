# Proiect-ABD

Project description - You are required to make several statistical computations on some US Zips dataset using MongoDB as the database platform.



Requirements:

a) Get the states with a total population of over 10 million.

b) Get the average city population by state.

c) Get the largest and the smallest city in each state.

d) Get the largest and the smallest counties in each state.

e) Get the nearest 10 zips from one of Chicago's landmarks, the Willis Tower situated at coordinates 41.878876, -87.635918.

f) Get the total population situated between 50 and 200 kms around New York's landmark, the Statue of Liberty at coordinates 40.689247, -74.044502.


First steps:

1) Conectarea la cluster-ul MongoDB Atlas:

mongosh "mongodb+srv://cluster0.dusbo.mongodb.net/abdtest" --username abdroot

![1](https://github.com/andra022/Proiect-ABD/assets/100848049/d4695981-fbbd-41cf-8061-35d15f03b45d)

2) Importarea dataset-ului US Zips

Convertirea fisierului de tip excel in format json + importarea acestuia.

mongoimport –uri "mongodb+srv://cluster0.dusbo.mongodb.net/abdtest" --username abdroot --password Test123 --collection zips --file uszips.json –jsonArray

![2](https://github.com/andra022/Proiect-ABD/assets/100848049/62159f34-dcba-49a6-b9a9-e46bf495d2cb)



3) Optimizarea Indexurilor

Crearea de indici pe câmpurile utilizate frecvent în interogări este o practică comună de optimizare a bazelor de date, care permite aplicațiilor să funcționeze mai rapid și să fie mai scalabile.

db.zips.createIndex({ state: 1 });
db.zips.createIndex({ city: 1 });
db.zips.createIndex({ county: 1 });

Am utilizat 1 pentru a specifica faptul că indexul este creat în ordine crescătoare.

![3](https://github.com/andra022/Proiect-ABD/assets/100848049/75b2317d-6dde-4170-b72d-14706831572f)



Subpunctele proiectului:


a) Get the states with a total population of over 10 million

db.zips.aggregate([

  { $group: { _id: "$state", totalPopulation: { $sum: "$population" } } },

  { $match: { totalPopulation: { $gt: 10000000 } } }

]);


Documentele se grupează după câmpul state, calculând populația totală pentru fiecare stat utilizând operatorul $sum. 

Apoi, se aplică operatorul $match pentru a filtra statele cu o populație totală mai mare de 10 milioane.

![a)](https://github.com/andra022/Proiect-ABD/assets/100848049/24922257-5276-4023-8f32-2efd146ebc2a)



b) Get the average city population by state

db.zips.aggregate([

  { $group: { _id: { state: "$state_name", city: "$city" }, cityPopulation: { $sum: "$population" } } },

  { $group: { _id: "$_id.state", avgCityPopulation: { $avg: "$cityPopulation" } } }

]);

Documentele se grupează după state și city, calculând populația totală pentru fiecare oraș. 

Ulterior, rezultatele se grupează după state, calculând populația medie a orașelor utilizând operatorul $avg.


![b)](https://github.com/andra022/Proiect-ABD/assets/100848049/778e6b31-27f0-4c7a-9fcf-ee792c633d9c)



c) Get the largest and smallest city in each state

db.zips.aggregate([

  { $group: { _id: { state: "$state_name", city: "$city" }, cityPopulation: { $sum: "$population" } } },
  
  { $sort: { "_id.state": 1, cityPopulation: 1 } },
  
  {
  
    $group: {
    
      _id: "$_id.state",
      
      largestCity: { $last: "$_id.city" },
      
      smallestCity: { $first: "$_id.city" }
      
    }
    
  }
  
]);

Documentele sunt grupate după câmpurile state și city, calculând populația totală pentru fiecare oraș. 

Apoi, rezultatele sunt sortate în funcție de state și populația orașului. 

După aceea, rezultatele sunt grupate din nou după state, iar din fiecare grup sunt selectate primul și ultimul oraș, reprezentând cel mai mic și cel mai mare oraș din fiecare stat.

![c)](https://github.com/andra022/Proiect-ABD/assets/100848049/6519a063-b072-4edf-b8b7-966c7a7f2f78)
