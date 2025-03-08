-- Notify the player that the script has loaded
local StarterGui = game:GetService("StarterGui")
StarterGui:SetCore("SendNotification", {
    Title = "Void Walker Loaded";
    Text = "Jump into the void to activate air walking!";
    Duration = 5;
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

local isWalkingOnAir = false
local trail = nil

-- Function to create a cool trail effect
local function createTrail()
    if trail then
        trail:Destroy()
    end

    trail = Instance.new("Trail")
    trail.Parent = humanoidRootPart

    local attachment0 = Instance.new("Attachment", humanoidRootPart)
    local attachment1 = Instance.new("Attachment", humanoidRootPart)

    trail.Attachment0 = attachment0
    trail.Attachment1 = attachment1
    trail.Color = ColorSequence.new(Color3.fromRGB(0, 255, 255), Color3.fromRGB(0, 0, 255)) -- Blue gradient
    trail.Lifetime = 0.5
    trail.Transparency = NumberSequence.new(0.2, 1)
    trail.LightEmission = 0.8
    trail.Enabled = true
end

-- Function to detect when the player falls into the void
local function checkVoid()
    while true do
        task.wait(0.1) -- Check every 100ms

        if humanoidRootPart.Position.Y < -5 and not isWalkingOnAir then
            -- Player is below the map and falling into the void
            isWalkingOnAir = true
            humanoid.PlatformStand = true -- Disable ragdoll
            humanoid:ChangeState(Enum.HumanoidStateType.Physics) -- Override gravity

            createTrail() -- Create the trail effect

            -- Lift player up slightly every 10ms
            while isWalkingOnAir do
                if humanoidRootPart.Position.Y > 5 then
                    -- Stop when the player reaches land
                    isWalkingOnAir = false
                    humanoid.PlatformStand = false
                    humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)

                    if trail then
                        trail.Enabled = false
                    end
                else
                    -- Slowly rise in the air while in the void
                    humanoidRootPart.Velocity = Vector3.new(0, 2, 0) 
                end
                task.wait(0.01) -- Every 10 milliseconds
            end
        end
    end
end

-- Run the void detection loop
task.spawn(checkVoid)
