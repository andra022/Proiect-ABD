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



