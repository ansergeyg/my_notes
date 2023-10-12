LANGUAGE VERSIONING:

https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/configure-language-version


How to change framework versions using dotnet command:

dotnet new console -o my_proj_name --framework net7.0

dotnet new console -o my_proj_name --framework netcoreapp3.1


When you have a struct you're not sharing it with anyone
so it's fine for you to mutate it. You're not hurting anyone else.
When you have an object that is of a class value it can be shared with someone and be mutated so it has cosequences for others.
So it's much safer to have mutable structs then it is to have mutable objects.
Though it can be tricky. Probably because of the performance issues (copy-time)?

Source: https://youtu.be/D8-jIdLKCdA?t=2309
