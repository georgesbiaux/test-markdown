#Importation

Click on the button below to download the latest VoodooSauce Lite unity package:

[DOWNLOAD BUTTON w/ version number]()

Then, import the unity package in your project by:

- Going in your unity project and then: `Assets > Import Package > Custom Package...` and browse to the downloaded unity package.

Or

- Double click on the unity package file while having your unity project open.

An Import window should open, click on the `Import` button to finish the importation.

---

#Configuration

Once you've imported your unity package, a new menu should appear called `VoodooSauce`, click on it and select `Edit Settings`, this will select the settings asset.

Fill the 5 fields using theses ids:

| Name | Key |
| ------------- |:-------------:|
| GameAnalytics key | ... |
| GameAnalytics build secret | ... |
| Appodeal App Key | ... |
| VidCoin Game Id | ... |
| VidCoin Placement Id | ... |

After that, you can click on the `Enable Cross Promo` button in order to activate the Cross promotion.

---

#SDK Integration

Add the following method calls to your code:

### Game

Add this call whenever the player starts a new round/level:

```csharp
VoodooSauce.OnGameStarted();
```

Add this call whenever the player loses or wins a round/level:

```csharp
VoodooSauce.OnGameFinished();
```

### Ads

#### Banner

Add this call when your application starts, the banner should be visible as soon as possible, and should always remain visible. You can pass a callback when the banner has been loaded and is being shown using the `onComplete` parameter. The float given to the callback will be the height of the banner in pixels (almost always `50`). The parameter is optionnal, you can pass `null` if you don't need it.

```csharp
VoodooSauce.ShowBanner(Action<float> onComplete);
```

If in some situations you need to hide the banner, you can use the following method:

```csharp
VoodooSauce.HideBanner();
```

**Note**: The banner will always show at the bottom of the screen. Make sure your bottom UI is not blocked by the banner, and nothing blocks the banner.

#### Interstitial

The interstitial is a 5 second short ad, it should be displayed at the end of every rounds/levels. The conditions to display the interstitial are handled internally.
Add the following call after the player finishes a level/round. It is recommended to show an interstitial after a user input (i.e. when he press *Continue* or *Try Again*), and the previous `OnGameFinished` method should be called **before** showing the interstitial.
The parameter `onComplete` is a callback that will be called after the interstitial has been watched or if the conditions to display the ads weren't met. This parameter is optionnal, you can pass `null` if you don't need it.

```csharp
VoodooSauce.ShowInterstitial(Action onComplete);
```

#### Rewarded Video

The rewarded video is a 30 second long ad that will reward the player if he watches it until the end. This ad should only be displayed when the player wants to watch it. It is often used for **second life** or **bonuses**.
Use the following method to display such ad. The callback `onComplete` is mandatory, as it will be used whenever the rewarded video has been closed by the player. The `bool` passed to the callback will be `true` if the player has watched the ad until the end, if the player closed the ad before the end, it will be `false`.

```csharp
VoodooSauce.ShowRewardedVideo(Action<bool> onComplete);
```

**Example**

```csharp
public void GivePlayerSecondChance()
{
	VoodooSauce.ShowRewardedVideo((completed) =>
	{
		if (completed)
		{
			// Give the user the second chance
		}
		else
		{
			// Don't do anything
		}
	});	
}
```

When you need to display a button for second chance or bonuses, you can check if a rewarded video has been loaded and is ready to play by calling the following method:

```csharp
VoodooSauce.IsRewardedVideoAvailable();
```

**Example**

```csharp
public void GameOver()
{
	if (VoodooSauce.IsRewardedVideoAvailable())
	{
		// Show second chance button
	}
	else
	{
		// Hide second chance button
	}
}
```

### GDPR Compliance

Our package includes a built-in GDPR compliance system that will display a popup at the starts of the application. This is handled automatically.
However there is one thing that should be done manually. You should add a `edit privacy policy` button somewhere in your game, in your settings menu for example.
Whenever the player clicks on the button, you should call the following method:

```csharp
VoodooSauce.ShowGDPRSettings(Action onSettingsClosed);
```

This will display a popup that allows the user to remove his consent. Whenever the player closes the popup, the callback `onSettingsClosed` is called. If you don't need the callback you can pass `null`.

As the GDPR is only available in Europe, the button should not be displayed if the user is not concerned by that law. You can retrieve such information by calling the following method:

```csharp
VoodooSauce.RequestGdprApplicability(Action<bool> callback);
```

**Example**

```csharp
// Script to place on the button game object
void Start()
{
	// Hide the button by default
	gameObject.SetActive(false);

	VoodooSauce.RequestGdprApplicability((isApplicable) => 
	{
		if (isApplicable)
		{
			// Gdpr is applicable in player's country, show the button
			gameObject.SetActive(true);
		}
		else
		{
			// Do nothing, button already hidden
		}
	});
}
```
