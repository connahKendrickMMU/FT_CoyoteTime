# FT_CoyoteTime
In this focus topic you will work on a key aspect of the assignment, gameplay iteration and refinement. You'll look a a common place mechanic in platformers, Coyote Time. 

This is a relativitly easy one that you will be able to do,but the focus is on seeing a mechanic refinement and planning how oyu can shwo this in your assignment.

## Overview
Try to implement the mechanic using the following hints to adjust the CoyoteTime Character scripts:
You will need to rebind input in C++, implement 3 functions and add a tick function

## Where to find the relevant scripts

For this project the main focus is the CoyoteTime Character scripts.

## Task
- Implement Coyote Time into the character movement system, allowing the character a slight buffer to jump while falling.
## Hints  
### Header Hints

For a good implementation of Coyote Time  the following variables in the header file would help, notice there is a new jump function:
```
public:
	ACoyoteTimeCharacter();
	// New jump function for Coyote time
	void CoyoteJump();
	void Landed(const FHitResult& Hit) override;
	void Tick(float DeltaTime) override;

protected:
	// track the Coyote time jump
	float CoyoteTimeDuration = 0.2f; // coyote time jump limit
	float TimeSinceLeftGround = 0.0f;
	bool bCanUseCoyoteTime = false;
```
Likewise you will want a function that you can call in tick passing the delta time into to track how long the charcater has been in the air for:
```
// function to update and track our Coyote Time Window
void UpdateCoyoteTime(float DeltaTime);
```

### CPP Hints
In the check for the jump you will want to expand to see if the character, can jump with an or to Coyote Time:
```
void ACoyoteTimeCharacter::CoyoteJump()
{
	if (GetCharacterMovement()->IsMovingOnGround() || bCanUseCoyoteTime)
	{
		// Reset coyote time
		bCanUseCoyoteTime = false;
		TimeSinceLeftGround = 0.0f;

		// Perform the jump
		LaunchCharacter(FVector(0.0f, 0.0f, GetCharacterMovement()->JumpZVelocity), false, true);
	}
}
```
You will need to reset everything for in the landed function to ensure you reset the jump:
```
void ACoyoteTimeCharacter::Landed(const FHitResult& Hit)
{
	Super::Landed(Hit);

	// Reset coyote time variables
	TimeSinceLeftGround = 0.0f;
	bCanUseCoyoteTime = false;
}
```
Then you will need the good that actually tracks and makes a decision on the Coyote Time jump availability:
```
void ACoyoteTimeCharacter::UpdateCoyoteTime(float DeltaTime)
{
	if (!GetCharacterMovement()->IsMovingOnGround())
	{
		TimeSinceLeftGround += DeltaTime;

		if (TimeSinceLeftGround <= CoyoteTimeDuration)
		{
			bCanUseCoyoteTime = true;
		}
		else
		{
			bCanUseCoyoteTime = false;
		}
	}
	else
	{
		// Player is on the ground, reset variables
		TimeSinceLeftGround = 0.0f;
		bCanUseCoyoteTime = false;
	}
}
```

Don't forget you will need to call the **UpdateCoyoteTime** from the **Tick** methods, and like wise you should **rebind** the input from the standard jump to **CoyoteJump**.
## Changes from base project
The changes in this topic are all minor.

## Challenges

This is one our oddest challenges, most of our tasks is around researching theory and implementation of a game design concepts and the key phrases "Gameplay Iteration", "Gameplay Refinement" and "Gameplay Juice". This is more then just making a machanic, it about making a mechanic that players enjoy using without even realising it. - 
![If you do thing right, people won't be sure you've done anything at all](Hints/futurama.gif)

MORTAL KOMBAT!

### Easy
- For most this is a newer concept, however, games have a array refinements. Look into these, as well as other game refinements, such as the "Rule of 40 Seconds", "Recognition over Recall" and "Juice/ Feel". 
- See a video videos from [Game Juice](https://www.youtube.com/watch?v=Fy0aCDmgnxg) on it, look at how packages, like [Feel by More Mountains](https://feel.moremountains.com/) for Unity build on these concepts.
- Watch this in how they made a [Button](https://www.youtube.com/watch?v=7L1B5YaxxoA) more fun.

### Medium
- Coyote time expands just jumping, in [An Elaborate History of Chess](https://store.steampowered.com/app/1255020/An_Elaborate_History_of_Chess/) It is used on the GUI to allow selection when pieces are moving. See where else you can use it in your game.
- Look at the game [Celeste](https://store.steampowered.com/app/504230/Celeste/) they make precise changes to jump frames such as the jumping up vs drop, for precise gameplay design. They also make use of Coyote Time.
- Look at [Helldivers 2](https://store.steampowered.com/app/553850/HELLDIVERS_2/) they make the shooting more weighty and less smooth with an additional reticule to show the off set. The goal was to make the gameplay feel more gritty to fit the theme.
- Explore the [content examples](https://www.fab.com/listings/4d251261-d98c-48e2-baee-8f4e47c67091) to see allthe feature of unreal. Epic give guideance on [how to explore them](https://dev.epicgames.com/documentation/en-us/unreal-engine/content-examples-sample-project-for-unreal-engine).

### Hard - Your turn
- Focus on what mechanic you want to develop.
- How can you refine and iterate on that mechanic.
- Talk with me, the helpers, all of your colleauges and friends on your idea and how to refine it. 
# Reference
Content is made by Connah Kendrick (Connah.Kendrick@mmu.ac.uk) using the Unreal Engine Third Person Template for the MMU 1st year Game Mechanics Module taught to both Game Development and Game Design Students. 