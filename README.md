## Class Library with Static Resource Library

Ok, let's say our MAUI class library provides three custom label types and an extended button.

[![library][1]][1]

If we add "Resources.xaml" as a New Item using the **.NET MAUI Resource Dictionary (XAML)** template we can add some values:

```xaml
<ResourceDictionary 
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:Library"
    x:Class="Library.Resources">
    
    <x:String x:Key="DefaultText">My Text</x:String>

    <Style
        x:Key="BaseStyleEx"
        TargetType="View" >
        <Setter Property="HeightRequest" Value="50"  />
        <Setter Property="WidthRequest" Value="150" />
    </Style>
    <Style
        x:Key="BaseLabelStyleEx"
        TargetType="local:LabelEx"
        BasedOn="{StaticResource BaseStyleEx}">
        <Setter Property="VerticalTextAlignment" Value="Center" />
        <Setter Property="HorizontalTextAlignment" Value="Center" />
    </Style>
    <Style TargetType="local:LabelEx" BasedOn="{StaticResource BaseLabelStyleEx}">
        <Setter Property="BackgroundColor" Value="WhiteSmoke" />
    </Style>
    <Style TargetType="local:RedLabel" BasedOn="{StaticResource BaseLabelStyleEx}">
        <Setter Property="BackgroundColor" Value="LightSalmon" />
    </Style>
    <Style TargetType="local:GreenLabel" BasedOn="{StaticResource BaseLabelStyleEx}">
        <Setter Property="BackgroundColor" Value="LightGreen" />
    </Style>
    <Style TargetType="local:ButtonEx" BasedOn="{StaticResource BaseStyleEx}">
        <Setter Property="BackgroundColor" Value="WhiteSmoke" />
        <Setter Property="TextColor" Value="Maroon" />
        <Setter Property="CornerRadius" Value="20" />
        <Setter Property="BorderColor" Value="DarkGray" />
        <Setter Property="BorderWidth" Value="2" />
    </Style>
</ResourceDictionary>
```

___

**Linking**

Since this class library is not contained by a common `Application` class, in order to link it make a reference in the xaml for any class that wants to use it.

###### LabelEx

```xaml
<Label
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Library.LabelEx"
    Text="{StaticResource DefaultText}">
    <Label.Resources>
        <ResourceDictionary Source="Resources.xaml"/>
    </Label.Resources>
</Label>
```

_In this case, since `RedLabel` and `GreenLabel` inherit `LabelEx` this single reference will suffice._

###### ButtonEx

```xaml
<Button 
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Library.ButtonEx"    
    Text="{StaticResource DefaultText}">
    <Button.Resources>
        <ResourceDictionary Source="Resources.xaml"/>
    </Button.Resources>
</Button>
```
___

**External App**

Now modify the default Maui app to use the Class Library.

[![demo][2]][2]

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:library="clr-namespace:Library;assembly=Library"
             x:Class="Client.MainPage">
    <ScrollView>
        <VerticalStackLayout
            Spacing="25"
            Padding="30,0"
            VerticalOptions="Center">

            <Image
                Source="dotnet_bot.png"
                SemanticProperties.Description="Cute dot net bot waving hi to you!"
                HeightRequest="200"
                HorizontalOptions="Center" />

            <library:LabelEx
                SemanticProperties.HeadingLevel="Level2"
                SemanticProperties.Description="Welcome to dot net Multi platform App U I"
                FontSize="18" />

            <library:RedLabel
                SemanticProperties.HeadingLevel="Level2"
                SemanticProperties.Description="Welcome to dot net Multi platform App U I"
                FontSize="18" />

            <library:GreenLabel
                SemanticProperties.HeadingLevel="Level2"
                SemanticProperties.Description="Welcome to dot net Multi platform App U I"
                FontSize="18" />

            <library:ButtonEx
                x:Name="CounterBtn"
                SemanticProperties.Hint="Counts the number of times you click"
                Clicked="OnCounterClicked"
                HorizontalOptions="Center" />

        </VerticalStackLayout>
    </ScrollView>
</ContentPage>
```
 


  [1]: https://i.stack.imgur.com/anDtm.png
  [2]: https://i.stack.imgur.com/M5drg.pngur.com/DRX7p.png