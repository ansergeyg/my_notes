```cs
        public static void myMthedo()
        {
            int a1 = 4;
            int a2 = 8;

            // We cannot use ref/out with Action types so this doesn't work
            Action<int, int> swap2 = (int a, int b) => { int t = a; a = b; b = t; };
            swap2(a1, a2);

            // swap(ref a1, ref a2);
            Console.WriteLine("a1={0} a2={1}", a1, a2);                

            //Local method inside method
            void swap(ref int a, ref int b)
            {
                int t = a;
                a = b;
                b = t;
            }
        }

```


-----------------------------------------------------------------



LANGUAGE VERSIONING:

https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/configure-language-version

-----------------------------------------------------------------


How to change framework versions using dotnet command:


dotnet new console -o my_proj_name --framework net7.0

dotnet new console -o my_proj_name --framework netcoreapp3.1

-----------------------------------------------------------------

Disable verbose logging of symbol loading in vscode debug console :

```cs
"logging": {
    "moduleLoad": false
}
```

https://stackoverflow.com/questions/55683834/disable-verbose-logging-of-symbol-loading-in-vscode-debug-console

-----------------------------------------------------------------
Source: https://stackoverflow.com/questions/61666697/how-to-build-executables-for-specific-platforms-in-net


If you are using .NET Core 3.x or later, you can use dotnet publish with various options to produce stand-alone executables:

dotnet publish -c Release -r linux-x64 -p:PublishSingleFile=true

You will need to re-run this command for each platform (Windows, macOS, Linux) that you want to target:

dotnet publish -c Release -r win-x64 -p:PublishSingleFile=true

dotnet publish -c Release -r osx-x64 -p:PublishSingleFile=true

-p:PublishSingleFile=true produces a single executable that you can copy around and run. You can attach it as an artifact with the release on GitHub.

(Aside: This executable will automatically extract itself into a few dozen files and then run that version. That means that you wont be able to run it, for example, on a system without a disk that you can write to.)

And yes, you can definitely wrap this up in a Makefile or something. Here's an example from a cli tool that I work on:


-----------------------------------------------------------------

Thoughts on mutating structs or classes:

When you have a struct you're not sharing it with anyone
so it's fine for you to mutate it. You're not hurting anyone else.
When you have an object that is of a class value it can be shared with someone and be mutated so it has cosequences for others.
So it's much safer to have mutable structs then it is to have mutable objects.
Though it can be tricky. Probably because of the performance issues (copy-time)?

Source: https://youtu.be/D8-jIdLKCdA?t=2309
