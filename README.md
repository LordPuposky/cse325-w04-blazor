# CSE 325 - Week 04: Build Web Applications with Blazor

Tutorial work for the Microsoft Learn path [Build web applications with Blazor](https://learn.microsoft.com/en-us/training/paths/build-web-apps-with-blazor/).

**Environment:** .NET SDK 10.0.404 · Visual Studio Code · Windows

## Repository structure

```
cse325-w04-blazor/
├── Week04Blazor.slnx
├── 01-FormsValidation/
│   └── BlazingPizza/            # Module 1
└── 02-ReusableComponents/
    ├── FirstClassLibrary/       # Module 2 - Razor class library (Modal component)
    └── MyBlazorApp/             # Module 2 - Blazor app that consumes the library
```

## Module 1 - Improve how forms and validation work in Blazor web apps

| Unit | What was done                                                                                                                                                                                                                                                                                                                  |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 3    | Split the checkout page into `OrderReview` and `AddressEditor` components, wired the _Place order_ button with `@onclick`, and set focus on the Name field with `@ref` + `FocusAsync()`.                                                                                                                                       |
| 5    | Wrapped the checkout in an `EditForm`, replaced `<input>` with `<InputText>`, and added manual validation using `EditContext` in `OnSubmit`.                                                                                                                                                                                   |
| 7    | Added Data Annotations to `Address` (`Required`, `MinLength`, `MaxLength`, `RegularExpression`) with custom error messages, `DataAnnotationsValidator`, a `ValidationMessage` per field, `OnValidSubmit`/`OnInvalidSubmit`, and an `EditContext` with `OnFieldChanged` to disable the submit button while the form is invalid. |

**Run:**

```bash
dotnet run --project 01-FormsValidation/BlazingPizza
```

## Module 2 - Build reusable components with Blazor

| Unit | What was done                                                                                                                                                                                                                                                                          |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3    | Created `FirstClassLibrary` (Razor class library) with a reusable `Modal` component: `Title`, `CancelText`, `ConfirmText`, `ChildContent` (`RenderFragment`), `Show`, `OnCancel`/`OnConfirm` (`EventCallback`) and isolated CSS. Used it in `MyBlazorApp` through a project reference. |
| 5    | Added package metadata (`PackageId`, `Version`, `Authors`, `Company`, `Description`), generated `My.FirstClassLibrary.0.1.0.nupkg` with `dotnet pack`, and replaced the project reference with a NuGet package reference.                                                              |

**Run:** the `.nupkg` lives in `bin/` and is not committed, so generate it and restore from the local folder first:

```bash
cd 02-ReusableComponents/FirstClassLibrary
dotnet pack
cd ../MyBlazorApp
dotnet restore --source ../FirstClassLibrary/bin/Release --source https://api.nuget.org/v3/index.json
dotnet run
```

## Notes

- The tutorials target .NET 8 / .NET 9. All projects were created or upgraded to **.NET 10** (`net10.0`) to match the installed SDK.
- **BlazingPizza starter:** changed `TargetFramework` to `net10.0`, updated EF Core packages to `10.0.*`, removed `System.Net.Http.Json` (included in .NET 10), and fixed `dotnetRunMessages` in `launchSettings.json` (string `"true"` → boolean `true`).
- **FirstClassLibrary:** the .NET 10 template enables nullable reference types, so the tutorial code produces two `CS8618` warnings (`Title`, `ChildContent`). They don't affect behavior; the code was kept as in the tutorial.
