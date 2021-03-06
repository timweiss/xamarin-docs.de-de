---
title: Lokalisierung von rechts nach links
description: Durch die Lokalisierung von rechts nach links kann Text in der Leserichtung von rechts nach links in Xamarin.Forms-Anwendungen dargestellt werden.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: c4098bfe40a252da2adbe7a7a2cd4c0f105ad1c8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050412"
---
# <a name="right-to-left-localization"></a>Lokalisierung von rechts nach links

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)

_Durch die Lokalisierung von rechts nach links kann Text in der Leserichtung von rechts nach links in Xamarin.Forms-Anwendungen dargestellt werden._

> [!NOTE]
> Für die Lokalisierung von rechts nach links ist iOS 9 oder höher bzw. unter Android API 17 oder höher erforderlich.

Die Leserichtung ist die Richtung, in der Benutzeroberflächenelemente auf einer Seite vom Auge wahrgenommen werden. In einigen Sprachen wie Arabisch und Hebräisch werden Benutzeroberflächenelemente von rechts nach links geschrieben. Dieses Verhalten wird erreicht, indem Sie die Eigenschaft [`VisualElement.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) festlegen. Diese Eigenschaft ruft die Leserichtung von Benutzeroberflächenelementen ab oder legt diese fest, und zwar innerhalb aller übergeordneten Elemente, die das Layout steuern. Daher muss sie auf einen der [`FlowDirection`](xref:Xamarin.Forms.FlowDirection)-Enumerationswerte festgelegt werden:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Wenn Sie die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft für ein Element auf [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft) festlegen, wird die Ausrichtung in der Regel rechtsseitig und die Leserichtung und das Layout des Steuerelements von rechts nach links festgelegt:

[![TodoItemPage auf Arabisch mit einer Schreibrichtung von rechts nach links](rtl-images/TodoItemPage-Arabic.png "TodoItemPage auf Arabisch mit einer Schreibrichtung von rechts nach links")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage auf Arabisch mit einer Schreibrichtung von rechts nach links")

> [!TIP]
> Sie sollten nur die Eigenschaft [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) auf das anfängliche Layout festlegen. Wenn dieser Wert zur Laufzeit geändert wird, hat dies einen teuren Layoutvorgang zur Folge, der die Leistung beeinträchtigt.

Der Standardwert der [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft für ein Element ohne übergeordnetes Element lautet [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight), während der Standardwert von `FlowDirection` für ein Element mit übergeordnetem Element [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent) ist. Aus diesem Grund erbt ein Element den Wert der `FlowDirection`-Eigenschaft vom übergeordneten Element in der visuellen Struktur, und alle Elemente können den vom übergeordneten Element geerbten Wert auch überschreiben.

> [!TIP]
> Wenn Sie eine App für Sprachen mit einer Schreibrichtung von rechts nach links lokalisieren, legen Sie die Eigenschaft [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) für die Seite oder das Rootlayout fest. Dadurch werden alle Elemente auf der Seite oder im Rootlayout der Schreibrichtung entsprechend ausgerichtet.

## <a name="respecting-device-flow-direction"></a>Beachtung der Schreibrichtung des Geräts

Dass die Schreibrichtung auf dem Geräts der ausgewählten Sprache und Region entsprechend beachtet wird, ist eine bewusste Entscheidung der Entwickler und passiert nicht automatisch. Für die richtige Schreibrichtung muss die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft für eine Seite oder ein Rootlayout auf den Wert `static` [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection) festgelegt werden:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Alle untergeordneten Elemente auf der Seite oder im Rootlayout erben in diesem Fall standardmäßig den [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection)-Wert.

## <a name="platform-setup"></a>Plattformeinrichtung

Es ist eine bestimmte Plattformkonfiguration erforderlich, um die Gebietsschemas zu ermöglichen, in denen von rechts nach links gelesen wird.

### <a name="ios"></a>iOS

Das erforderliche Gebietsschema, in dem von rechts nach links gelesen wird, sollte als unterstützte Sprache zu den Arrayelementen für den `CFBundleLocalizations`-Schlüssel in **Info.plist** hinzugefügt werden. Im folgenden Beispiel wird gezeigt, wie dem Array für den `CFBundleLocalizations`-Schlüssel Arabisch hinzugefügt wurde:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Unterstützte Sprachen in „Info.plist“](rtl-images/ios-locales.png "Unterstützte Sprachen in „Info.plist“")

Weitere Informationen finden Sie unter [Grundlagen der Lokalisierung in iOS](https://docs.microsoft.com/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Die Lokalisierung von rechts nach links kann getestet werden, indem Sie die Sprache und die Region auf dem Gerät/Simulator in ein Gebietsschema ändern, in dem von rechts nach links gelesen wird und das in **Info.plist** angegeben wurde.

> [!WARNING]
> Beachten Sie, dass beim Ändern der Sprache und der Region in ein Gebietsschema, das von rechts nach links gelesen wird, unter iOS alle [`DatePicker`](xref:Xamarin.Forms.DatePicker)-Ansichten eine Ausnahme auslösen, wenn Sie die für das Gebietsschema erforderlichen Ressourcen nicht einfügen. Wenn Sie z. B. eine App auf Arabisch testen, die eine `DatePicker`-Ansicht hat, sollten Sie sicherstellen, dass **mideast** im Abschnitt **Internationalisierung** des Bereichs **iOS-Build** ausgewählt ist.

### <a name="android"></a>Android

Die Datei **androidmanifest.xml** der App sollte aktualisiert werden, damit der `<uses-sdk>`-Knoten das `android:minSdkVersion`-Attribut auf 17 und der `<application>`-Knoten das `android:supportsRtl`-Attribut auf `true` festlegt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Die Lokalisierung von rechts nach links lässt sich anschließend testen, indem Sie das Gerät/den Emulator so konfigurieren, dass die Sprache mit Schreibrichtung von rechts nach links verwendet wird. Stattdessen können Sie zum Testen auch unter **Einstellungen > Entwickleroptionen** die Option **Force RTL layout direction** (Richtung des RTL-Layouts erzwingen) aktivieren.

### <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Die erforderlichen Sprachressourcen sollten im `<Resources>`-Knoten der Datei **Package.appxmanifest** angegeben sein. Im folgenden Beispiel wird gezeigt, wie auf dem `<Resources>`-Knoten Arabisch hinzugefügt wird:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Darüber hinaus erfordert die UWP, dass die Standardkultur der Anwendung explizit in der .NET Standard-Bibliothek definiert ist. Legen Sie dazu das `NeutralResourcesLanguage`-Attribut in `AssemblyInfo.cs` oder einer anderen Klasse wie folgt auf die Standardkultur fest:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Die Lokalisierung von rechts nach links kann getestet werden, indem Sie die Sprache und die Region auf dem Gerät in das richtige Rechts-nach-Links-Gebietsschema ändern.

## <a name="limitations"></a>Einschränkungen

Für die Lokalisierung von rechts nach links gelten in Xamarin.Forms derzeit die folgenden Einschränkungen:

- Die Platzierung der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)-Schaltfläche, die Platzierung der Symbolleistenelemente und die Übergangsanimation werden durch das Gebietsschema des Geräts und nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft gesteuert.
- Die Wischrichtung von [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) wird nicht umgestellt.
- Der visuelle Inhalt von [`Image`](xref:Xamarin.Forms.Image) wird nicht umgestellt.
- Die Ausrichtung von [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) und [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Der Inhalt von [`WebView`](xref:Xamarin.Forms.WebView) berücksichtigt nicht die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Für die Steuerung der Textausrichtung muss eine `TextDirection`-Eigenschaft hinzugefügt werden.

### <a name="ios"></a>iOS

- Die Ausrichtung von [`Stepper`](xref:Xamarin.Forms.Stepper) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Die Textausrichtung von [`EntryCell`](xref:Xamarin.Forms.EntryCell) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Gesten und die Ausrichtung von [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) werden nicht umgekehrt.

### <a name="android"></a>Android

- Die Ausrichtung von [`SearchBar`](xref:Xamarin.Forms.SearchBar) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Die Platzierung von [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.

### <a name="uwp"></a>UWP

- Die Textausrichtung von [`Editor`](xref:Xamarin.Forms.Editor) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.
- Die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft wird nicht von den untergeordneten [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)-Elementen geerbt.
- Die Textausrichtung von [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) wird durch das Gebietsschema des Geräts gesteuert, nicht durch die [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)-Eigenschaft.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Unterstützung für Sprachen in der Xamarin.University, die von rechts nach links gelesen werden

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 Right-to-Left Support (Rechts-nach-Links-Unterstützung in Xamarin.Forms 3.0), [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App „TodoLocalizedRTL“](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
