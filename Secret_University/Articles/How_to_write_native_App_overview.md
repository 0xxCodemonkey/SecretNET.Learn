# How to build a native cross-platform mobile app with Secret.NET and .NET MAUI

In this article you will learn how to build a native cross-platform mobile app with Secret.NET and .NET MAUI.

In the end you will have an mobile App that can run on Android, iOS and even on a desktop PC on Windows and macOS. 

**You will learn how to:**

- create an cross-platform mobile app with .NET MAUI
- setup a Secret.NET client and connect to Secret Network
- create an integrated wallet
- fund the wallet via testnet faucet
- query balance and send $SCRT coins
- initialize a secret contract
- interacting and query a secret contract

# Table of Contents

- Current options for building a secret mobile application
    - JavaScript
    - .NET / C#
        - Secret.NET
            - Additional packages
        - What is .NET MAUI?
            - Who .NET MAUI is for
            - More information about .NET MAUI
- Getting started
    - Prerequisites
    - Create a .NET MAUI project and app
    - Install Secret.NET and other useful nuget packages
    - Build the application
        - Setup the Secret.NET client
        - Create a Wallet
        - Create the UI
        - Add functionality
            - Wallet
            - Smart Contract
        - Run the app in the android simulator
- Congratulations

# Current options for building a secret mobile application 
If you want to write an cross-platform mobile application that works with Secret Network you have the choice of using **JavaScript** or **.NET / C#** (which is the focus of this article).


## JavaScript

For JavaScript you can use one of the JavaScript based Frameworks like [NativeScript](https://nativescript.org/), [React Native](https://reactnative.dev/) or [Ionic](https://ionicframework.com/) for building the application and use [secretjs](https://github.com/scrtlabs/secret.js) the official JavaScript Library for Secret Network. In addition there is also [Griptape](https://docs.griptapejs.com/), a web framework for developing dApps on Secret Network.

One of the disadvantages of this approach is that the user can only use the mobile application through an extra wallet application, such as [StarShell](https://starshell.net/) or [Fina](https://fina.cash/). 

An advantage here is certainly that often large parts of an existing web application can be reused.

## .NET / C#

.NET / C# is another way to build a true native cross-platform mobile app for Android and iOS and use [Secret.NET](https://github.com/0xxCodemonkey/SecretNET) ([nuget](https://www.nuget.org/packages/SecretNET)) a community .NET Library for Secret Network.

One advantage here, in contrast to the JavaScript solution, is that Secret.NET has an integrated wallet and thus runs independently without an external wallet application, thus enhancing the user experience. For the storage of the private key, the most secure method is used for each platform, e.g. the KeyChain under iOS.

### Secret.NET
[Secret.NET](https://github.com/0xxCodemonkey/SecretNET) is a .NET Library for interacting with Secret Network. The Library is written in .NET 6 and supports MAUI.

Secret.NET is a full port of [secretjs](https://github.com/scrtlabs/secret.js) and e.g. supports every possible message and transaction type and handles input/output encryption/decryption for Secret Contracts.

![](../resources/SecretNET.png)

#### Additional packages
In addition the following complementary packages are available, which act as a layer on top of the Secret.NET:
- [**SecretNET.Token**](https://github.com/0xxCodemonkey/SecretNET.Token) supports all methods of the [reference implementation](https://github.com/scrtlabs/snip20-reference-impl) of the [**SNIP20 contract**](https://docs.scrt.network/secret-network-documentation/development/snips/snip-20-spec-private-fungible-tokens).

- [**SecretNET.NFT**](https://github.com/0xxCodemonkey/SecretNET.NFT) supports all methods of the [reference implementation](https://github.com/baedrik/snip721-reference-impl) of the [**SNIP721 contract**](https://docs.scrt.network/secret-network-documentation/development/snips/snip-721-private-non-fungible-tokens-nfts).

### What is .NET MAUI?

**.NET Multi-platform App UI (.NET MAUI)** is a cross-platform framework for creating native mobile and desktop apps with C# and XAML.

Using .NET MAUI, you can **develop apps that can run on Android, iOS, macOS, and Windows from a single shared code-base**.

.NET MAUI is open-source and is the evolution of Xamarin.Forms, extended from mobile to desktop scenarios, with UI controls rebuilt from the ground up for performance and extensibility. 

Using .NET MAUI, you can create multi-platform apps using a single project, but **you can add platform-specific source code and resources if necessary**. One of the key aims of .NET MAUI is to enable you to implement as much of your app logic and UI layout as possible in a single code-base.

#### Who .NET MAUI is for

.NET MAUI is for developers who want to:
- Write cross-platform apps in XAML and C#, from a single shared code-base in Visual Studio.
- Share UI layout and design across platforms.
- Share code, tests, and business logic across platforms.

#### More informations about .NET MAUI: 
- [What is .NET MAUI?](https://learn.microsoft.com/en-us/dotnet/maui/what-is-maui)
- [Learn how to use .NET MAUI to build apps that run on mobile devices and on the desktop using C# and Visual Studio.](https://learn.microsoft.com/en-us/training/paths/build-apps-with-dotnet-maui/)
- [Resources to Get Started with .NET MAUI](https://devblogs.microsoft.com/dotnet/learn-dotnet-maui/)
- [.NET MAUI for Beginners video series](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oUBAdL2NwBpDs32zwGqb9DY)
- [.NET MAUI documentation](https://learn.microsoft.com/en-us/dotnet/maui/?WT.mc_id=dotnet-35129-website&view=net-maui-7.0)
- [Enterprise Application Patterns Using .NET MAUI](https://learn.microsoft.com/en-us/dotnet/architecture/maui/)

# Getting started

## Prerequisites
You will need [Visual Studio 2022 (17.4)](https://visualstudio.microsoft.com/downloads/) (not Visual Studio Code)  with the .NET MAUI workload installed and you should be familiar with C# and .NET. 

Since this is primarily an article about SecretNET and not about C# / .NET I will only briefly point out the most important basics here. If you want to learn more about C# I suggest you to start [here](https://dotnet.microsoft.com/en-us/learn/csharp).

If you don't know .NET MAUI I definitely recommend you to go through the learning path [here!](https://learn.microsoft.com/en-us/training/paths/build-apps-with-dotnet-maui/), watch the [.NET MAUI for Beginners video series](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oUBAdL2NwBpDs32zwGqb9DY) or check the [.NET MAUI documentation](https://learn.microsoft.com/en-us/dotnet/maui/?WT.mc_id=dotnet-35129-website&view=net-maui-7.0) and the [Enterprise Application Patterns Using .NET MAUI](https://learn.microsoft.com/en-us/dotnet/architecture/maui/).

The [**GitHub repository of the application final application**](https://github.com/0xxCodemonkey/SecretNET.Examples/tree/main/src/SimpleSecretMauiApp) can be used as reference.

## Create a .NET MAUI project and app
To create a new **.NET MAUI App** project with Visual Studio, in the Create a new project dialog box, select the .NET MAUI project type, and then choose the .NET MAUI App template:

![](../resources/3-create-maui-app.png)

Follow the steps in the wizard to name the project and specify a location.
Select **.NET 7.0** as the target framework:

![](../resources/3-create-maui-app_net7.png)

A newly created .NET MAUI project contains the items as shown:

![](../resources/3-new-solution.png)

You can find more details about the project structure / resources and the application startup [here](https://learn.microsoft.com/en-us/training/modules/build-mobile-and-desktop-apps/3-create-a-maui-project-visual-studio?ns-enrollment-type=learningpath&ns-enrollment-id=learn.dotnet-maui.build-apps-with-dotnet-maui).

To check that everything works, run the app by pressing F5.

## Install Secret.NET and other useful nuget packages
Next we load the Secret.NET library and other useful nuget packages.

For Secret.NET, simply search for "Secret Network" in the NuGet Package Manager and install the latest version of the **SecretNET** package:

![](../resources/install_secretnet_nuget.png)

Additionally we need the [**.NET MAUI Community Toolkit**](https://github.com/CommunityToolkit/Maui)(search "CommunityToolkit.Maui") and the [**.NET MVVM Community Toolkit**](https://github.com/CommunityToolkit/dotnet)(search "CommunityToolkit.Mvvm").

![](../resources/install_communitytoolkit_nuget.png)

Then we need to register the MVVM Toolkit in the **MauiAppBuilder** ([MauiProgram.cs](https://github.com/0xxCodemonkey/SecretNET.Examples/blob/main/src/SimpleSecretMauiApp/SimpleSecretMauiApp/MauiProgram.cs#L13)):

```csharp
var builder = MauiApp.CreateBuilder();
		builder
			.UseMauiApp<App>()
            .UseMauiCommunityToolkit()
            .ConfigureFonts(fonts =>
			{
				fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
				fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
			});
```

## Build the application
Now we are ready to start building the app.

**The app should:**

- connect to Secret Network
- have a integrated wallet
- can query the balance of the wallet
- can send transactions respectively send $SCRT coins
- can init, interact and query a secret smart contract.

**A .NET MAUI project initially contains**:

- The ``MauiProgram.cs`` file that contains the code for **creating and configuring the Application** object.
- The ``App.xaml`` and ``App.xaml.cs`` files that provide **UI resources** and create the initial window for the application.
- The ``AppShell.xaml`` and ``AppShell.xaml.cs`` files that specify the **initial page** for the application and handle the **registration of pages for navigation routing**.
- The ``MainPage.xaml`` and ``MainPage.xaml.cs`` files that define the layout and UI logic for the page displayed by default in the initial window.

### Setup the Secret.NET client
To set up the Secret.NET client, we create an instance and register it for [dependency injection in the MauiAppBuilder](https://learn.microsoft.com/en-us/dotnet/architecture/maui/dependency-injection). 

For the Secret.NET client we need to configure the **gRPC-web URL** and the **ChainID**, which can be found in [here](https://docs.scrt.network/secret-network-documentation/development/connecting-to-the-network) in the Secret Network Documentation. 

In this example we will also enable the feature ``AlwaysSimulateTransactions`` to simulate all transactions before sending, to get the estimated gas fees. However on mainnet it's recommended to not simulate every transaction as this can burden your node provider, and you will do it normally upfront and set it fix in your code.

Since we will create the wallet in the next step, we will omit it here for now and thus create a read-only instance.

And because we need to register more classes later, we offload this to a separate method ``RegisterViewsAndViewModels``, which we then also call in MauiAppBuilder.

```csharp
public static void RegisterViewsAndViewModels(MauiAppBuilder builder)
	{
        // Setup Secret.NET client
        var gprcUrl = "https://pulsar-2.api.trivium.network:9091";
        var chainId = "pulsar-2";

        // client options
        var createClientOptions = new CreateClientOptions(gprcUrl, chainId, wallet: null) 
        {
            AlwaysSimulateTransactions = true,
        };

        var secretClient = new SecretNetworkClient(createClientOptions);

        // register the instance as singleton
        builder.Services.AddSingleton<SecretNetworkClient>(secretClient);
    }
```

To later create a wallet to interact with Secret Network, we also need to create a ``IPrivateKeyStorage`` that specifies where we want to securely store our private key and register it. In a .NET MAUI app, this should be the **MauiSecureStorage** provider.

The .NET MAUI SecureStorage uses the OS capabilities as follows to store the data securely:

| Plattform | Info |
| ------------- | -------------  |
| Android | Data is encrypted with the Android EncryptedSharedPreference Class, and the secure storage value is encrypted with AES-256 GCM. |
| iOS / macOS | The iOS Keychain is used to store values securely. | 
| Windows | DataProtectionProvider is used to encrypt values securely on Windows devices. | 

:warning: **Never store the private key or mnemonic phrase permanent in a variable (or somewhere else than in a secure storage) or output them in a log!**

So to create and register the ``IPrivateKeyStorage`` we add the following lines to the ``RegisterViewsAndViewModels`` method:

```csharp
builder.Services.AddSingleton<IPrivateKeyStorage>(new MauiSecureStorage());
```

### Create the UI
Next, we first create our user interface / UI, which we then extend with functionality.

Our app should have **two tabs**:

- Wallet
- Smart Contract

To create these tabs first need to create two empty XAML pages. For this we create an folder "Pages" where we put our pages and add a "**.NET MAUI ContentPage (XAML)**" named "**WalletPage.xaml**" and "**SmartContractPage.xaml**":

![](../resources/add_page.png)

Next, we create the tabs on the **initial page** (``AppShell.xaml``) by replacing the ``ShellContent`` Element with a [``TabBar``](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/shell/tabs?view=net-maui-7.0) Element.

To set the content page of each tab we use [XAML Data Binding](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/data-binding/?view=net-maui-7.0) and need to register the namespace of our pages:

```xml
<Shell
    x:Class="SimpleSecretMauiApp.AppShell"
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:SimpleSecretMauiApp"
    xmlns:pages="clr-namespace:SimpleSecretMauiApp.Pages"
    Shell.FlyoutBehavior="Disabled">

    <TabBar>
        <Tab Title="Wallet">
            <ShellContent Route="WalletPage" ContentTemplate="{DataTemplate pages:WalletPage}" />            
        </Tab>
        <Tab Title="Smart Contract">
            <ShellContent Route="SmartContractPage" ContentTemplate="{DataTemplate pages:SmartContractPage}" />
        </Tab>
    </TabBar>

</Shell>
```

By hitting ``F5`` you can every time check if everything works so far.

#### Wallet Page UI
On our Wallet Page we want to show the current wallet address and balance or give the user the option to create a wallet.

We need the following controls on the WalletPage for this:

- ``Button`` to create a new wallet
- ``Label`` for the wallet address
- ``Label`` for the wallet balance
- ``Button`` to query the balance

To keep things simple, here we use the [VerticalStackLayout](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/layouts/verticalstacklayout?view=net-maui-7.0) for the arrangement of our controls.

Inside the ``VerticalStackLayout`` on the ``WalletPage.xaml`` we place our controls (we control the visibility of the controls later, when we add the functionality).

```xml
<VerticalStackLayout>

    <Button Text="Create wallet" HorizontalOptions="Center" Margin="0,0,0,10" />

    <Label Text="Wallet Address" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10"/>
    <Label Text="Wallet Balance" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10" />
    <Button Text="Query balance" HorizontalOptions="Center" />

</VerticalStackLayout>
```

:information_source: While debugging the application you can edit the XAML File, e.g. change the margin, and see immediately the result in the running app, thanks to [XAML Hot Reload](https://learn.microsoft.com/en-us/dotnet/maui/xaml/hot-reload?view=net-maui-7.0&tabs=vswin). But XAML Hot Reload doesn't reload C# code, including event handlers.

Our app looks now something like this:

![](../resources/wallet_page_v1.png)

#### Smart Contract Page UI
On our Smart Contract Page we want to first init a secret smart contract and then interact with the contract (query and execute methods).

For this example we will use the SECRET COUNTER contract from the [SECRET BOX](https://scrt.university/repositories/secret-box/secret-counter). 

We need the following controls on the SmartContractPage for this:

- ``Button`` to init a secret smart contract instance 
- ``Label`` for the current counter value
- ``Button`` to query the counter
- ``Button`` to increment the counter

Inside the ``VerticalStackLayout`` on the ``SmartContractPage.xaml`` we place our controls (we control the visibility of the controls later, when we add the functionality).

```xml
<VerticalStackLayout>

    <Button Text="Init Contract" HorizontalOptions="Center" Margin="0,0,0,10" />

    <Label Text="Current Counter" FontSize="30" HorizontalTextAlignment="Center" Margin="0,20,0,20"/>
    <Button Text="Query Counter" HorizontalOptions="Center" Margin="0,0,0,10" />
    <Button Text="Increment Counter" HorizontalOptions="Center" />

</VerticalStackLayout>
```

### Add functionality
Now that we have created the UI, let's add functionality to the app.

To separate the logic / code from the UI we use the [Model View ViewModel pattern (MVVM)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel), and for this in particular the [MVVM Community Toolkit](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/).

So we will create **a ViewModel for each View / Page**, where all our functionality will be placed. To keep things simple and focus on the functionality of Secret.NET we will combine the ViewModel and the Model in one class.

With MVVM you use **DataBinding to bound properties** of the view model to the UI and use **commands to call methods** on the view model.

The view model inherits from the [**ObservableObject**](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/observableobject) class in the MVVM Toolkit and every property that is needed for the UI gets exposed with a [**ObservableProperty**](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/generators/observableproperty) attribute and every method with a [**RelayCommand**](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/generators/relaycommand) attribute.

#### Wallet
Let's start with the view model for our wallet page. 

Create an folder "ViewModel" and add a new class ``WalletViewModel.cs``.
Make the class public, partial and inherit from ``ObservableObject``:

```csharp
namespace SimpleSecretMauiApp.ViewModel
{
    public partial class WalletViewModel : ObservableObject
    {
    }
}
```
We need the following properties and methods exposed for the WalletPage:

- property ``Address`` for the wallet address
- property ``Balance`` for the wallet balance
- property ``HasWallet`` which indicates that we already have a wallet
- method ``CreateWallet``  to create a new wallet
- method ``GetBalance`` to query the balance

Thanks to the MVVM Toolkit attributes **ObservableProperty** and **RelayCommand**, which generates boilerplate code for us, we save and typing. 

**For properties** to work, we must follow the **rule of noting properties** as **private and lowercase**. This then results in an uppercase Observable property on the view model.

**For commands** the resulted command name will be the method name with subsequent "Command".

For our ``WalletViewModel`` this looks like this:

```csharp
public partial class WalletViewModel : ObservableObject
{
    [ObservableProperty]
    private string address = "-";

    [ObservableProperty]
    private float balance = 0;

    [ObservableProperty]
    private bool hasWallet = false;

    [RelayCommand]
    public async void CreateWallet()
    {
        // TODO
    }

    [RelayCommand]
    public async void GetBalance()
    {
        // TODO
    }
}
```

##### Create a Wallet
Next, we create a wallet that we can later use to interact with Secret Network. 

Let's start with avoiding the creating of a new wallet every time the application is launched, we check beforehand if there is already one. We will implement it in a method ``Init()`` which we call in the constructor where we also get the instance of our ``SecretNetworkClient`` and the ``IPrivateKeyStorage``:

```csharp
private SecretNetworkClient _secretClient;
private IPrivateKeyStorage _privateKeyStorage;
private CreateWalletOptions _createWalletOptions;

public WalletViewModel(SecretNetworkClient secretClient, IPrivateKeyStorage privateKeyStorage)
{
    _secretClient = secretClient;
    _privateKeyStorage = privateKeyStorage;
    _createWalletOptions = new CreateWalletOptions(_privateKeyStorage);
    Init();
}

private async void Init(IPrivateKeyStorage privateKeyStorage)
{
    if (await privateKeyStorage.HasPrivateKey())
    {
        var createWalletOptions = new CreateWalletOptions(privateKeyStorage);
        var wallet = await Wallet.Create(await privateKeyStorage.GetFirstMnemonic(), options: createWalletOptions);

        // attach the wallet to the secretClient instance
        _secretClient.Wallet = wallet;
        Address = wallet.Address;
        HasWallet = true;
    }
}
```

Next we implement the ``CreateWallet`` method.

In this example we will create a brand new wallet that we will later need to fund through [testnet faucet](https://faucet.pulsar.scrttestnet.com/).
Normally you will give the user the additional option to setup the wallet with their own **mnemonic phrase** e.g. from an existing keplr wallet.

```csharp
[RelayCommand]
public async void CreateWallet()
{
    if (!HasWallet)
    {
        var wallet = await Wallet.Create(options: _createWalletOptions);
        _secretClient.Wallet = wallet;
        Address = wallet.Address;
        HasWallet = true;
    } 
}
```

In order to use our view model on our WalletPage, we have to **register our page and view model** in the ``RegisterViewsAndViewModels``as well and **get and assign an instance on the page**.

``MauiProgram.cs``

```csharp
builder.Services.AddSingleton<WalletPage>();
builder.Services.AddSingleton<WalletViewModel>();
```

``WalletPage.xaml.cs``

```csharp
public partial class WalletPage : ContentPage
{
	public WalletPage(WalletViewModel viewModel)
	{
        BindingContext = viewModel;
        InitializeComponent();
	}
}
```

Please note that we set the ``BindingContext`` of the page to the **view model**. This way we bound it to the page and can use it for data binding.

Now let's bind our properties and commands to the WalletPage via [Data Binding](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/data-binding/?view=net-maui-7.0):

```xml
<VerticalStackLayout>

    <Button Text="Create wallet" Command="{Binding CreateWalletCommand}" HorizontalOptions="Center" Margin="0,0,0,10" />

    <Label Text="{Binding Address}" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10"/>
    <Label Text="{Binding Balance, StringFormat='{0:F2} SCRT'}" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10" />
    <Button Text="Query balance" Command="{Binding GetBalanceCommand}" HorizontalOptions="Center" />

</VerticalStackLayout>
```

If you now run the app by hitting ``F5``, you should be able to create a new wallet by clicking the button. 

If everything works you should see your brand new wallet address in the label and you can copy the new address out of the debug / output log:

![](../resources/create_wallet.png)

Use this address in the [testnet faucet](https://faucet.pulsar.scrttestnet.com/) to fund the wallet with some $SCRT.

Now we need to implement the ``GetBalance`` method, so that we can query the balance of our wallet.
In the method we need also to convert the amount from uSCRT to SCRT.

```csharp
[RelayCommand]
public async void GetBalance()
{
    if (HasWallet)
    {
        var balanceResponse = await _secretClient.Query.Bank.Balance(Address);
        if (balanceResponse?.Amount != null)
        {
            float amount;
            if (float.TryParse(balanceResponse.Amount, out amount))
            {
                Balance = amount / 1000000;
            }
        }
    }
}
```

Call the ``GetBalance`` method also in ``Init`` if we already have an wallet, so the balance is shown right after starting the app.

Finally we should hide the "Create wallet" button if there we have a wallet and show the "Query balance" button and labels and vice versa. For the invert of the ``HasWallet`` property will need a [value converter](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/data-binding/converters?view=net-maui-7.0) called ``InvertedBoolConverter`` from the MAUI Toolkit. For this we need to register the namespace of the converter and the converter in the page resources:

```xml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:mct="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"
             x:Class="SimpleSecretMauiApp.Pages.WalletPage"
             Title="WalletPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <mct:InvertedBoolConverter x:Key="InvertedBoolConverter" />
        </ResourceDictionary>
    </ContentPage.Resources>
```

and the bind the ``IsVisible`` property of the controls to the ``HasWallet`` property:

```xml
<Button Text="Create wallet" Command="{Binding CreateWalletCommand}" IsVisible="{Binding HasWallet, Mode=OneWay, Converter={StaticResource InvertedBoolConverter}}" HorizontalOptions="Center" Margin="0,0,0,10" />

<Label Text="{Binding Address}" IsVisible="{Binding HasWallet}" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10"/>
<Label Text="{Binding Balance, StringFormat='{0:F2} SCRT'}" IsVisible="{Binding HasWallet}" FontSize="15" HorizontalTextAlignment="Center" Margin="0,0,0,10" />
<Button Text="Query balance" Command="{Binding GetBalanceCommand}" IsVisible="{Binding HasWallet}" HorizontalOptions="Center" />
```

#### Smart Contract


For this example we will use the SECRET COUNTER contract from the [SECRET BOX](https://scrt.university/repositories/secret-box/secret-counter). 

:information_source: **Secret.NET unfortunately cannot be connected to localsecret (Docker) yet**, as the docker image currently does not provide an encrypted connection on gRPC-web port 9091.
As far as I know, .NET cannot be connected to an unencrypted port via gRPC-web unless it offers HTTP/2 exclusively, which is not the case with localsecret (it also runs HTTP 1.1 on port 9091). See [here](https://learn.microsoft.com/en-us/aspnet/core/grpc/troubleshoot?view=aspnetcore-6.0#call-insecure-grpc-services-with-net-core-client) and [here](https://learn.microsoft.com/en-us/aspnet/core/grpc/aspnetcore?view=aspnetcore-6.0&tabs=visual-studio#protocol-negotiation).

### Run the app in the android simulator 


=> Beispiel App mit Navigation & Smart Contract => aus Projekt entnehmen und Repro verlinken => https://github.com/0xxCodemonkey/SecretNET.Examples

## Secret Interaktionen
=> Setup Wallet
=> Secret Queries & Transaktionen
=> Upload & Init Contract (Contract aus Uni nehmen!)
=> Interact mit Contract

# Congratulations

Congratulations, you have build your first secret native cross-platform mobile application :)

# Additional resources
