-- DR-SILENO HUB V3 (SAFE)

local KEY = "Edu55456"
local PREFIX = ";"

local p = game.Players.LocalPlayer
local ts = game:GetService("TweenService")
local rs = game:GetService("RunService")
local light = game:GetService("Lighting")

local gui = Instance.new("ScreenGui", game.CoreGui)

-- TWEEN
local function tween(o,props,t)
	ts:Create(o,TweenInfo.new(t,Enum.EasingStyle.Quad,Enum.EasingDirection.Out),props):Play()
end

-- LOGIN
local login = Instance.new("Frame",gui)
login.Size = UDim2.new(0,320,0,180)
login.Position = UDim2.new(0.5,-160,-200,0)
login.BackgroundColor3 = Color3.fromRGB(80,0,120)

local keyBox = Instance.new("TextBox",login)
keyBox.Size = UDim2.new(0.8,0,0,40)
keyBox.Position = UDim2.new(0.1,0,0.4,0)
keyBox.PlaceholderText = "Digite a key..."

local status = Instance.new("TextLabel",login)
status.Size = UDim2.new(1,0,0,30)
status.Position = UDim2.new(0,0,0.75,0)
status.BackgroundTransparency = 1

tween(login,{Position=UDim2.new(0.5,-160,0.3,0)},0.6)

-- LOADING
local load = Instance.new("Frame",gui)
load.Size = UDim2.new(0,320,0,120)
load.Position = UDim2.new(0.5,-160,1,0)
load.BackgroundColor3 = Color3.fromRGB(80,0,120)
load.Visible=false

local txt = Instance.new("TextLabel",load)
txt.Size = UDim2.new(1,0,0,40)
txt.Text = "Painel rapido Dr-sileno"
txt.TextColor3 = Color3.new(1,1,1)
txt.BackgroundTransparency = 1

local bar = Instance.new("Frame",load)
bar.Size = UDim2.new(0,0,0,10)
bar.Position = UDim2.new(0,0,1,-10)
bar.BackgroundColor3 = Color3.fromRGB(0,255,100)

-- MAIN
local main = Instance.new("Frame",gui)
main.Size = UDim2.new(0,360,0,300)
main.Position = UDim2.new(0.5,-180,-300,0)
main.BackgroundColor3 = Color3.fromRGB(90,0,140)
main.Visible=false
main.Active=true
main.Draggable=true

local title = Instance.new("TextLabel",main)
title.Size = UDim2.new(1,0,0,40)
title.Text = "Dr-sileno hub"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1

-- PARTICULAS
for i=1,25 do
	local pt = Instance.new("Frame",main)
	pt.Size = UDim2.new(0,3,0,3)
	pt.BackgroundColor3 = Color3.fromRGB(0,255,100)
	pt.BorderSizePixel = 0
	
	task.spawn(function()
		while true do
			pt.Position = UDim2.new(math.random(),0,1,0)
			tween(pt,{Position=UDim2.new(math.random(),0,0,0)},math.random(3,6))
			task.wait(3)
		end
	end)
end

-- COMMAND BOX
local cmd = Instance.new("TextBox",main)
cmd.Size = UDim2.new(1,-20,0,40)
cmd.Position = UDim2.new(0,10,0,45)
cmd.PlaceholderText = ";comando"

-- MENU
local menu = Instance.new("Frame",main)
menu.Size = UDim2.new(1,-20,0,200)
menu.Position = UDim2.new(0,10,0,90)
menu.BackgroundColor3 = Color3.fromRGB(40,0,60)

-- BOTÃO
local function btn(txt,y,func)
	local b = Instance.new("TextButton",menu)
	b.Size = UDim2.new(1,0,0,25)
	b.Position = UDim2.new(0,0,0,y)
	b.Text = txt
	b.BackgroundColor3 = Color3.fromRGB(0,255,100)
	b.MouseButton1Click:Connect(func)
end

-- FUNÇÕES SEGURAS
function Speed(v)
	if p.Character then
		p.Character.Humanoid.WalkSpeed = v
	end
end

function Jump(v)
	if p.Character then
		p.Character.Humanoid.JumpPower = v
	end
end

function Reset()
	if p.Character then
		p.Character:BreakJoints()
	end
end

function FPS()
	light.GlobalShadows=false
	settings().Rendering.QualityLevel="Level01"
end

-- BOTÕES
btn("Speed 50",0,function() Speed(50) end)
btn("Jump 100",25,function() Jump(100) end)
btn("Reset",50,Reset)
btn("FPS Boost",75,FPS)

-- COMANDOS
cmd.FocusLost:Connect(function(e)
	if not e then return end
	
	local text = cmd.Text:lower()
	cmd.Text=""
	
	if text:sub(1,1) ~= PREFIX then return end
	
	local args = text:sub(2):split(" ")
	local c = args[1]
	
	if c=="speed" then Speed(tonumber(args[2]))
	elseif c=="jump" then Jump(tonumber(args[2]))
	elseif c=="reset" then Reset()
	elseif c=="fps" then FPS()
	end
end)

-- LOGIN CHECK
keyBox.FocusLost:Connect(function()
	if keyBox.Text == KEY then
		login.Visible=false
		load.Visible=true
		
		tween(load,{Position=UDim2.new(0.5,-160,0.4,0)},0.5)
		tween(bar,{Size=UDim2.new(1,0,0,10)},2)
		
		task.wait(2)
		load.Visible=false
		main.Visible=true
		
		tween(main,{Position=UDim2.new(0.5,-180,0.3,0)},0.6)
	else
		status.Text="Invalid key"
		status.TextColor3=Color3.new(1,0,0)
	end
end) 
--// =========================
--// 🤖 IA PRO DR-SILENO
--// Cole isso no FINAL do script
--// =========================

local IA_ENABLED = true

-- respostas base
local IA_DB = {
	["oi"] = "Salve 😎 sou a IA do Dr-sileno hub",
	["help"] = "Comandos: ;speed ;jump ;reset ;fps ;ui ;effect",
	["fps"] = "Dica: use ;fps para melhorar desempenho",
	["speed"] = "Use ;speed 50 para correr mais rápido",
	["jump"] = "Use ;jump 100 para pulo alto",
}

-- gerador de scripts
local function IAGenerateScript(msg)
	msg = msg:lower()
	
	if msg:find("gui") or msg:find("interface") then
		return [[
-- GUI básica
local f = Instance.new("Frame", game.CoreGui)
f.Size = UDim2.new(0,200,0,100)
f.Position = UDim2.new(0.5,-100,0.5,-50)
f.BackgroundColor3 = Color3.fromRGB(120,0,200)
]]
		
	elseif msg:find("botao") then
		return [[
-- Botão
local b = Instance.new("TextButton", game.CoreGui)
b.Size = UDim2.new(0,150,0,50)
b.Text = "Clique"
b.MouseButton1Click:Connect(function()
	print("Clicou!")
end)
]]
		
	elseif msg:find("particula") then
		return [[
-- Partículas simples
local p = Instance.new("ParticleEmitter", workspace.Terrain)
p.Rate = 50
p.Lifetime = NumberRange.new(1)
]]
	end
	
	return nil
end

-- resposta inteligente
local function IAResponse(msg)
	msg = msg:lower()
	
	-- banco direto
	for k,v in pairs(IA_DB) do
		if msg:find(k) then
			return v
		end
	end
	
	-- gerar script
	local script = IAGenerateScript(msg)
	if script then
		return script
	end
	
	-- fallback inteligente
	if msg:find("como") then
		return "Tente usar comandos como ;speed, ;jump ou ;fps"
	end
	
	return "Não entendi 🤔 tente: gui, botao, particula, fps..."
end

-- UI IA
local aiBox = Instance.new("TextBox")
aiBox.Parent = main
aiBox.Size = UDim2.new(1,-20,0,35)
aiBox.Position = UDim2.new(0,10,1,-45)
aiBox.PlaceholderText = "Pergunte pra IA..."

local aiOut = Instance.new("TextLabel")
aiOut.Parent = main
aiOut.Size = UDim2.new(1,-20,0,40)
aiOut.Position = UDim2.new(0,10,1,-90)
aiOut.BackgroundTransparency = 1
aiOut.TextColor3 = Color3.fromRGB(0,255,100)
aiOut.TextWrapped = true

-- animação de texto
local function typeText(label, text)
	label.Text = ""
	for i = 1, #text do
		label.Text = string.sub(text,1,i)
		task.wait(0.01)
	end
end

-- execução
aiBox.FocusLost:Connect(function(enter)
	if not enter or not IA_ENABLED then return end
	
	local input = aiBox.Text
	aiBox.Text = ""
	
	local response = IAResponse(input)
	
	typeText(aiOut, response)
end)
