# Garrys mod SpawnMenu

Merhaba, bugün studio'da luau ile garry's mod spawn menu yapacağım

## Garry's Mod SpawnMenu:

![2766979314_preview_GmodSMenu](https://github.com/user-attachments/assets/4f2ced2e-5b0a-44ba-83b8-2afe046deba9)
(kaynak https://steamcommunity.com/sharedfiles/filedetails/?id=2766979314)

## Basitten başlayacağım (gui)

önceden bunu yapmaya çalışmıştım, ama çok fazla dağınık çalıştığım için beceremedim. ama bu sefer klasörler ile çalışacağım ama mantıklı davranmalıyım.
ilk önce ScreenGui oluşturdum ve adını SpawnMenu koydum. bunu UserInputService hizmeti ile bağlayıp Q harfine basıldığında menünün kapanmasını sağlayacağım.

![image](https://github.com/user-attachments/assets/b959f47a-85c4-44b3-91bc-c9daf78fcf8e)


eğer UserInputService hizmetinin ne olduğunu bilmiyorsanız bununla ilgili bir makale yazdım. [UserInputService](https://github.com/NATO4100/luau-turkce-dokumasyon/blob/main/UserInputService.md)

Yazdığım kod tam olarak (şimdilik) bu

```lua
local UIS = game:GetService("UserInputService")
local SpawnMenu = script.Parent

UIS.InputBegan:Connect(function(input, gameProcessedEvent)
	if input.KeyCode == Enum.KeyCode.Q then
		SpawnMenu.Enabled = true
		print("Menü Açıldı!")
	end
end)

UIS.InputEnded:Connect(function(input, gameProcessedEvent)
	if input.KeyCode == Enum.KeyCode.Q then
		SpawnMenu.Enabled = false
		print("Menu kapandı!")
	end	
end)
```

https://github.com/user-attachments/assets/fb298e0d-defd-435a-84fa-7356c4576337

## gui'yi güzelleştirmek

evet gui gerçekten çok kötü gözüküyor şuan, GMod'daki spawnmenuye benzer bir şey yapmaya çalıştım.

örnek olarak sadece 1 prop ekledim.

![image](https://github.com/user-attachments/assets/5c733181-299e-46b4-ab4c-88f1fff16113)

sokak lambasını ReplicatedStorage hizmetinin içine koydum'ki oradan butona bastığımızda oyunumuza nesneyi klonyalabilelim.

![image](https://github.com/user-attachments/assets/06ec4ca4-3763-4754-b943-abad73343505)

## Prop spawn sistemi

evet en kafa yakan yere az kaldı, burada ReplicatedStore ve Players hizmetlerini kullanacağız, ve ayrıca CFrame.

test etmek amaçlı bu kodu yazdım

```lua
local RS = game:GetService("ReplicatedStorage")
local spawnButton = script.Parent
local Players = game:GetService("Players")

spawnButton.MouseButton1Click:Connect(function()
	local Humanoid = Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
	local lamba = RS["Street lamp"]:Clone()
	lamba.Parent = game.Workspace
end)
```
https://github.com/user-attachments/assets/fbfe8cfb-1fdb-4cb5-96eb-6680df158fc4

gördüğünüz gibi nesne uzakta spawn oluyor, bunu düzeltmek için CFrame kullanacağız.

yazdığım kod;

```lua
local RS = game:GetService("ReplicatedStorage")
local spawnButton = script.Parent
local Players = game:GetService("Players")

spawnButton.MouseButton1Click:Connect(function()
	local Humanoid = Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
	local Streetlamp = RS.Streetlamp:Clone()
	Streetlamp:PivotTo(Humanoid.CFrame * CFrame.new(1,5,-10))
	Streetlamp.Parent = game.Workspace

end)
```

teker teker kodu açıklamayıp şunu söyleyeceğim, PivotTo kullanarak humanoidRootPart'ımızın karşınıza doğru nesnemizi spawn ettik. işte video:

https://github.com/user-attachments/assets/01408c73-7f44-4dbc-b4ed-23858b2859ef

## şimdilik ara vereceğim, bunu geliştirdiğimde commit atacağım.
