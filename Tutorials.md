# Francesca_Chapman_Programming_Tutorials

## Tutorial 1: Top Down Movement

### 1.
Find your player idle Sprite in the Assets Folder, and add it to your scene. Before you do this, select the Sprite Asset and make sure the Pixels Per Unit setting in the Inspector is at an appropriate size so that your player is not too large or too small.


### 2.
With your player Object in the Scene selected, add a Rigidbody 2D Component in the Inspector. Set the Gravity Scale to 0 so the player won't fall down when you hit play. Under Constraints, tick the Freeze Rotation Z box so the player won't rotate unnecessarily.

Add a Box Collider 2D Component as well. Adjust the Size and Offset to fit the Sprite and make sure the Is Trigger box is unticked.

### 3.
Create a Script called PlayerMovement and attach it to the player Object. Open the Script in Visual Studio and delete the Start function as you don't need it. Add the following variables:

```
public float moveSpeed = 5f;
public Rigidbody2D rb;
Vector2 movement;
```

Save the Script and return to Unity. On your player underneath the Script Component, click and drag the Rigidbody Component into the rb reference.


### 4.
In your Script, underneath the Update function, add the FixedUpdate function as it is more reliable for physics-based actions, updating at 50 times a second regardless of framerate. Inside the Update function, add the following:

```
void Update()
{
    movement.x = Input.GetAxisRaw("Horizontal");
    movement.y = Input.GetAxisRaw("Vertical");
}
```

These will detect the direction you press on the keyboard and give you -1 for left or down, 1 for right or up, and 0 for no input. The movement variable will allow you to access these in the FixedUpdate function.


### 5.
Still in your Script, inside the FixedUpdate function, add the following:

```
void FixedUpdate()
{
    rb.MovePosition(rb.position + movement * moveSpeed * Time.fixedDeltaTime);
}
```

This will move the player's Rigidbody using the current position and the movement input from the Update function, moving at the speed set in the moveSpeed variable. Time.fixedDeltaTime is used to ensure that the speed stays the same however many times FixedUpdate is called.

Save your Script and press play in the Editor to test the movement with WASD or the arrow keys; your player should be moving according to the directional input.


### 6.
Go to the Window bar and find Animation>Animation. With the player selected, in the Animation window click the Create button in the centre to create an Animator component for the player as well as your first Animation.

Find an appropriate location to save the Animation, name it "Player_Idle_Front", and press Save. Within your Assets Folder, locate your idle Animation Frame(s), and drag them onto the Timeline of your Player_Idle_Front animation in the Animation window. Make 3 more Animations for the other directions with the corresponding Sprites.

If it is a single-frame Animation, you can move to the next step. If you have multiple Frames, space them out on the Timeline as you need, and change the Sample Rate if it is too fast or slow.


### 7.
Create a new Animation by clicking where it says Player_Idle and then Create New Clip. Name it Player_Walk_Up, then save. Add the Animation Frames for walking up by dragging them into the Timeline, like before. Check the Animation by pressing play; if it is too fast or slow, change the Sample Rate, which can be accessed by clicking the cog on the far right of the Animator window and clicking Show Sample Rate.

Repeat this for the remaining 3 Animations (left, right, down).


### 8.
Open the Animator window either through the Window bar or by clicking on your player's Animator in the Assets Folder. Delete all the Animation Nodes and right click>Create State>From New Blend Tree, and name it Idle. Make sure it is the default State - it should be orange, otherwise right click on it and select Set as Default Layer State. Underneath Parameters to the left in the Animator window, add 4 Floats, Horizontal, Vertical, lastHorizontal and lastVertical, and leave their values as 0.

Double click your Idle Node and click on the Blend Tree Node inside of it. At the top, change Blend Type to 2D Simple Directional, and underneath that change your 2 Parameters to lastHorizontal and lastVertical.


### 9.
Underneath Motion on the right, click the Plus button and click Add Motion Field 4 times. For each Motion Field add your idle Animations. For each Animation, set it so that:

Idle Front: Pos X = 0, Pos Y = -1

Idle Back: Pos X = 0, Pos Y = 1

Idle Left: Pos X = -1, Pos Y = 0

Idle Right: Pos X = 1, Pos Y = 0


### 10.
Go back to the Base Layer and create another Blend Tree (right click>Create State>From New Blend Tree) and call it Movement. Double click it and change the Blend Type to 2D Simple Directional and change the 2 Parameters to lastHorizontal and lastVertical.

Add 4 Motion Fields again, and add each of your walking Animations. Set them so that:

Walk Down: Pos X = 0, Pos Y = -1

Walk Up: Pos X = 0, Pos Y = 1

Walk Left: Pos X = -1, Pos Y = 0

Walk Right: Pos X = 1, Pos Y = 0

This will assign each Animation to the movement input directions referenced in the Script.


### 11.
At the top of the Animator window, click Base Layer to go back to your Idle and Movement Nodes. Add another Float Parameter under lastHorizontal and lastVertical, called Speed. Right click on the Idle Node, and select Make Transition, which should create a white arrow coming from it that you can attach to the Movement Node.

Once attached, click on the arrow and under Conditions on the right, click the Plus button to add a new Condition. The Condition should read: Speed Greater 0.01, meaning that the movement Animations will only play if the player is moving faster than that speed, which they will do when a directional input is given.

Above this, uncheck the box Has Exit Time, and expand the Settings underneath. Change the Transition Duration to 0.

Make another Transition from Movement to Idle, repeating the same steps as above, however, change the Condition to Speed Less 0.01, so that the movement Animations will stop when the player stops moving.


### 12.
Go back to your Script, and add the variable:

```
public Animator animator;
```

Underneath your movement code in the Update function, add:

```
animator.SetFloat("Horizontal", movement.x);
animator.SetFloat("Vertical", movement.y);
animator.SetFloat("Speed", movement.sqrMagnitude);

if (movement.x == 1 || movement.x == -1 || movement.y == 1 || movement.y == -1)
{
    animator.SetFloat("lastHorizontal", movement.x);
    animator.SetFloat("lastVertical", movement.y);
}
```

Save the Script and go back to the Editor. Select the player and underneath the Script Component, drag the Animator Component to the animator variable to assign it. Press play to test, and your player should display the walking Animations depending on which direction you press, and when it stops, they should display the idle Animation facing the same direction.

## Tutorial 2: Attack

### 1.
With your player selected, go to the Animation window and add 4 new clips for attack Animations in each direction. In your first clip, locate the corresponding Sprites and drag them into the timeline, making sure they are the right size to fit the other Sprites. Space them out on the Timeline so they animate at an appropriate speed. Select the newly made Animation and turn off Loop Time.

Repeat this for the other 3 animations.

### 2.
In the Animator window, delete the attack Animation Nodes that have appeared and create a new Blend Tree from right click>Create State>From New Blend Tree and rename it to Attack. Double click on it, change the Blend Type to 2D Simple Directional in the Inspector, and change the Parameters to lastHorizontal and lastVertical. Underneath Parameters on the left, add a new Trigger Parameter called Attack.

### 3.
Add 4 new Motion Fields in the Inspector and add each of your attack Animations for your player. Set each of them so that:

AttackDown: Pos X = 0, Pos Y = -1

AttackLeft: Pos X = -1, Pos Y = 0

AttackRight: Pos X = 1, Pos Y = 0

AttackUp: Pos X = 0, Pos Y = 1

### 4.
Return to your Base Layer in the Animator and right click on the PlayerIdle Node to select Make Transition, dragging the arrow to the Attack Node. Click on the Transition and untick Has Exit Time in the Inspector, drop down Settings and also untick Fixed Duration and set Transition Duration to 0. Underneath this, add 2 Conditions and change them so they say Attack, and lastHorizontal Greater 0.01. Add another Transition from PlayerIdle to Attack and set the Conditions to lastHorizontal Less -0.01 and Attack. Add 2 more Transitions like this, but set the Conditions to lastVertical Greater 0.01 and Attack, and lastVertical Less -0.01 and Attack. You should have 4 Transitions from the Idle Node to the Attack Node.

Right click on the Attack Node and make a Transition to PlayerIdle, this time keep Has Exit Time ticked, but untick Fixed Duration and change Transition Duration to 0. Add one last Transition from Movement to Attack, with the Condition Attack and no Exit Time like the previous Transitions. This allows the attack Animations to be played depending on the direction, whilst idle or whilst moving, and return to the previous state afterwards.

### 5.
Create a Script called Attack and add it to your player object in the Scene. Add these variables:

```
private float timeBtwAttack;
public float startTimeBtwAttack;
```
Remove the Start function as you do not need it, but under Update, add the following:

```
void Update()
{
    if (timeBtwAttack <= 0)
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            timeBtwAttack = startTimeBtwAttack;
        }
    }
    else
        {
            timeBtwAttack -= Time.deltaTime;
        }
}
```
This will allow the player to attack according to input, with a timer allowing some space before you can attack again after.

### 6.
Create 4 Empty Game Objects, and call them AttackPosRight, AttackPosLeft, AttackPosUp and AttackPosDown, and make them Children of the player. Make sure they have the same Transform Position as the player at the moment. Back to your Script, add these variables:

```
public Transform attackPosUp;
public Transform attackPosDown;
public Transform attackPosLeft;
public Transform attackPosRight;
    
public LayerMask whatIsEnemies;
public float attackRange;
public int damage;

public Animator animator;
```
And in your Update function, make the following changes to your Script:

```
if (timeBtwAttack <= 0)
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") > 0.01)
            {
                animator.SetTrigger("Attack");
                Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosUp.position, attackRange, whatIsEnemies);
                for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
                timeBtwAttack = startTimeBtwAttack;
            }
        }
    }
```
Copy and paste this 3 more times, and for each replace the Float and Float value in the If Condition to match the direction, and change AttackPosUp.position to the corresponding AttackPos Transforms. Each one should be:

```
if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") < -0.01)
    {
        animator.SetTrigger("Attack");
        Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosDown.position, attackRange, whatIsEnemies);
        for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
        timeBtwAttack = startTimeBtwAttack;
    }
    
if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") < -0.01)
    {
        animator.SetTrigger("Attack");
        Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosLeft.position, attackRange, whatIsEnemies);
        for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
        timeBtwAttack = startTimeBtwAttack;
    }
    
if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") > 0.01)
    {
        animator.SetTrigger("Attack");
        Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosRight.position, attackRange, whatIsEnemies);
        for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
        timeBtwAttack = startTimeBtwAttack;
    }
```
These changes are so that the attack detects what direction the player is at the time of input, and activates the correct Animation and Collider in that direction in order to hit whatever is in front of the player.

### 7.
Add a new Object to your scene, it can be an enemy or just something you want to be able to destroy. Give it a non-Trigger Box Collider 2D and Rigidbody 2D with a Gravity Scale of 0. Create a Script called Enemy, and add it to this Object. Add this variable:

```
public int health;
```
Delete the Start function and under Update, add this:

```
void Update()
{
    if (health <= 0)
    {
        Destroy(gameObject);
    }
}
```
Underneath this, create a new function called TakeDamage and add the following:

```
    public void TakeDamage(int damage)
    {
        health -= damage;
        Debug.Log("damage Taken");
    }
```
This will allow the enemy Object to take damage when the function is called in the Attack Script, lowering the health until it hits 0, where it will be destroyed.

### 8.
Back to your Attack Script, add the OnDrawGizmosSelected function underneath Update, and add this:

```
private void OnDrawGizmosSelected()
{
    Gizmos.color = Color.red;
    Gizmos.DrawWireSphere(attackPosUp.position, attackRange);
    Gizmos.DrawWireSphere(attackPosDown.position, attackRange);
    Gizmos.DrawWireSphere(attackPosLeft.position, attackRange);
    Gizmos.DrawWireSphere(attackPosRight.position, attackRange);
}
```
This simply allows you to see the Sphere for each Attack Position, allowing for easy adjustments.

### 9.
Save both of your Scripts and return to the Editor. Underneath the Attack Script on your player in the Hierarchy, set your variables so that StartTimeBtwAttack = 0.3, and drag and drop each of your AttackPos Objects to the matching AttackPos Transforms in the Script.

Select your Object, and underneath the Enemy Script, change the health to whatever you would like it to be, and change its Layer by clicking on the Layer tab at the top of the Inspector. If you do not have a Layer for your Object, create one by clicking Add Layer, and typing in a new Layer name in one of the empty slots. Click on the Object again and select the new Layer in the Layer tab to assign it. Back to your player Object, assign this new Layer in the WhatIsEnemies variable under the Attack Script.

Change the AttackRange to something reasonable; something relative to the player's size. Now that you can see each AttackPos, rearrange their Positions so they are on each side of the player according to their name. Finally, change the damage to 1 and drag the player's Animator Component to the Animator variable in the Attack Script.

### 10.
Go to your PlayerMovement Script, add this in your Update function:

```
if (animator.GetCurrentAnimatorStateInfo(0).IsName("Attack"))
    {
        movement = Vector2.zero;
    }
```
This will check if the player is in the Attack State in the Animator, and if it is then will stop the ability to move until the player leaves that State.

Save your Script and press play to test. Your player should trigger the attack Animation when the spacebar is pressed, unable to move until the Animation has ended. While still in Play Mode, select your enemy Object and check the health variable under the Enemy Script. When you attack with the enemy in range, you should see that the health goes down by 1 each time, and when it hits 0, the Object is destroyed.

## Tutorial 3: Health

### 1.
Select your player and go to the Animation window. Create 4 new Animations for when your player is dealt damage, one for each direction. In your first Clip, locate the corresponding Sprites and drag them into the Timeline, making sure they are the right size to fit the other Sprites. Space them out on the Timeline so they animate at an appropriate speed. Turn off Loop Time with the Animation selected.

Repeat this for the other Animations, and make 4 new Animations for when your player dies, and format them in the same way, but allow these to Loop.

### 2.
Delete the new Nodes in the Animator and create 2 new Trigger Parameters - Hurt and Death. Create your first of 2 new Blend Trees with right click>Create State>From New Blend Tree, and call it Hurt. Double click to open and set the Blend Type to 2D Simple Directional. Add 4 Motion Fields and add each of your 4 hurt Animations. Change Pos X and Pos Y so that:

HurtDown - Pos X: 0, Pos Y: -1

HurtLeft - Pos X: -1, Pos Y: 0

HurtRight - PosX: 1, Pos Y: 0

HurtUp - PosX: 0, Pos Y: 1

Create your second new Blend Tree and call it Death. Repeat the above steps for this, adding the death animations rather than hurt animations.

### 3.
Create a new Transition from Idle to Hurt and untick Has Exit Time and Fixed Duration, and change Transition Duration to 0. Add a new Condition and set it to the Hurt trigger. Create 2 new Transitions with the same settings from Movement to Hurt, and Attack to Hurt, and then another Transition from Hurt to Idle. For this Transition, keep Has Exit Time ticked, but untick Fixed Duration and change Transition Duration to 0.

Add one Transition from Hurt to Death, and give it a Condition with the Death Trigger.

### 4.
Create a new script called Health and attach it to the player. Add these variables:

```
public int maxHealth = 3;
public int currentHealth;
public Animator animator;
```
Under the Start function, add the following:

```
void Start()
{
    currentHealth = maxHealth;
}
```
Delete the Update function and add a new function called TakeDamage, like this:

```
public void TakeDamage(int amount)
{
    animator.SetTrigger("Hurt");
    currentHealth -= amount;

    if (currentHealth <= 0)
    {
        animator.SetTrigger("Death");
    }
}
```
This gives the player a set amount of health that will go down when hurt, and when it reaches 0, it kills the player.

### 5.
In your Enemy script, add the OnCollisionEnter2D function and add the following:

```
private void OnCollisionEnter2D(Collision2D collision)
{
    if (collision.gameObject.tag == "Player")
    {
        var healthComponent = collision.gameObject.GetComponent<Health>();
        if (healthComponent != null)
        {
            healthComponent.TakeDamage(1);
        }
    }
}
```
This allows the enemy to access and trigger the TakeDamage function on the player's script, so when they hit the player, the player's health decreases.

### 6.
Go to your PlayerMovement script add a new bool:

```
public bool inputEnable;
```
Make an edit to your main movement code to put it inside a new If Statement, like this:

```
if (inputEnable == true)
{
    movement.x = Input.GetAxisRaw("Horizontal");
    movement.y = Input.GetAxisRaw("Vertical");

    animator.SetFloat("Horizontal", movement.x);
    animator.SetFloat("Vertical", movement.y);
    animator.SetFloat("Speed", movement.sqrMagnitude);

    if (movement.x == 1 || movement.x == -1 || movement.y == 1 || movement.y == -1)
    {
        animator.SetFloat("lastHorizontal", movement.x);
        animator.SetFloat("lastVertical", movement.y);
    }
}
```
Change the AnimatorStateInfo code so it says:

```
if (animator.GetCurrentAnimatorStateInfo(0).IsName("Idle") || animator.GetCurrentAnimatorStateInfo(0).IsName("Movement"))
{
    inputEnable = true;
}
else
{
    movement = Vector2.zero;
    inputEnable = false;
}
```
This is just so that if the player is hurt, dead or attacking, they cannot move, or change facing direction, as that shouldn't be possible when in those States.

### 7.
Go to your attack Script and put the code for Input and all 4 AttackPos in another If Statement, so it looks like this:

```
void Update()
{
    if (timeBtwAttack <= 0)
    {
        if (gameObject.GetComponent<PlayerMovement>().inputEnable == true)
        {
            if (Input.GetKeyDown(KeyCode.Space))
            {
                if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") > 0.01)
                {
                    animator.SetTrigger("Attack");
                    Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosUp.position, attackRange, whatIsEnemies);
                    for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
                    timeBtwAttack = startTimeBtwAttack;
                }

                if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") < -0.01)
                {
                    animator.SetTrigger("Attack");
                    Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosDown.position, attackRange, whatIsEnemies);
                    for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
                    timeBtwAttack = startTimeBtwAttack;
                }

                if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") < -0.01)
                {
                    animator.SetTrigger("Attack");
                    Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosLeft.position, attackRange, whatIsEnemies);
                    for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
                    timeBtwAttack = startTimeBtwAttack;
                }

                if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") > 0.01)
                {
                    animator.SetTrigger("Attack");
                    Collider2D[] enemiesToDamage = Physics2D.OverlapCircleAll(attackPosRight.position, attackRange, whatIsEnemies);
                    for (int i = 0; i < enemiesToDamage.Length; i++)
                    {
                        enemiesToDamage[i].GetComponent<Enemy>().TakeDamage(damage);
                    }
                    timeBtwAttack = startTimeBtwAttack;
                }
            }
        }
    }
    else
    {
        timeBtwAttack -= Time.deltaTime;
    }
}
```
This is so that when the player is hurt or dead, they cannot attack or kill the enemy anymore, as this should not happen either. Save your Script, return to the Editor and press Play. When your player runs into the enemy, the player's health should go down by 1 and trigger the hit Animation, and when you do this several times and the health reaches 0, the player should be dead and can no longer do anything.

## Tutorial 4: Shoot

### 1.
With your player selected, go to the Animation window and click 4 new Animations for when the player is shooting, one for each direction. Locate your Sprites and put them into your first Animation - make sure the Sprites are the right size and that the Animation Samples is the right amount. Select the Animation and turn off Loop Time. Repeat this for the other 3 Animations. 

### 2.
Go to the Animator window and delete the new Nodes that have appeared. Create a new Blend Tree with Right Click>Create State>From New Blend Tree and name it Shoot. Double click to open it and change the Blend Type to 2D Simple Directional. Change the Parameters to lastHorizontal and lastVertical and add in all 4 new shooting Animations. Change the values of Pos X and Pos Y so that:

ShootUp - Pos X: 0, Pos Y: 1

ShootDown - Pos X: 0, Pos Y: -1

ShootLeft - Pos X: -1, Pos Y: 0

ShootRight - Pos X: 1, Pos Y: 0

### 3.
Go back to your Base Layer and make a new Parameter Trigger called Shoot. Make a Transition from Idle to Shoot, turn off Has Exit Time and Fixed Duration and set Transition Duration to 0, and add the condition Shoot. Make another Transition from Shoot to Idle, keep Has Exit Time ticked but turn off Fixed Duration and change Transition Duration to 0.

Add another Transition from Movement to Shoot and format it like the Idle to Shoot Transition.

### 4.
Repeat this process with your bullet Object. Add your bullet to the Scene and make 2 sets of Animations, one standard one in each direction and another one for when it hits something, again in each direction. In the Animator create 3 Parameters; 2 floats, lastHorizontal and lastVertical, and a Trigger, Hit. Delete the Nodes and set up 2 Blend Trees; the Default State should be Flying, with your standard Animations, and the other state should be Hit.

In both Blend Trees the Blend Type should be set to 2D Simple Directional and the Parameters should be lastHorizontal and lastVertical. Add 4 Motion Fields and the corresponding Animations to the Blend Tree; each directional Animation will have the same Pos X and Pos Y as the same directional Animation for the player's Shoot Blend Tree. Make a Transition from Flying to Hit - turn off Has Exit Time, untick Fixed Duration and change Transition Duration to 0, and add the Condition Hit.

### 5.
Create 4 empty GameObjects as Children of your player and position them where you would like to shoot the bullets from, one in each direction. Select your bullet and give it a Rigidbody 2D, setting the Gravity Scale to 0, and a Collider 2D, whatever fits the bullet best, and changing the size to match it.

Click the bullet in the Hierarchy and drag it to your Assets Folder to make it a Prefab. Delete the bullet in the Scene.

### 6.
Create a new Script called Shoot and attach it to the player. Delete the Start method and add in these variables:

```
public Transform firePointUp;
public Transform firePointDown;
public Transform firePointLeft;
public Transform firePointRight;
    
public GameObject bulletPrefab;

public float bulletForce = 20f;

public Animator animator;
```
In Update, add the following:

```
void Update()
{
    if (Input.GetKeyDown(KeyCode.T))
    {
        Shooting();
    }
}
```
This will call the shooting function when the button is pressed.

### 7.
In your Script, add a new function for Shooting. Add this:

```
void Shooting()
{
    if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") > 0.01)
    {
        animator.SetTrigger("Shoot");
        GameObject bullet = Instantiate(bulletPrefab, firePointUp.position, firePointUp.rotation);
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
        bullet.GetComponent<Animator>().SetFloat("lastVertical", 1);
        rb.AddForce(firePointUp.up * bulletForce, ForceMode2D.Impulse);
    }

    if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastVertical") < -0.01)
    {
        animator.SetTrigger("Shoot");
        GameObject bullet = Instantiate(bulletPrefab, firePointDown.position, firePointDown.rotation);
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
        bullet.GetComponent<Animator>().SetFloat("lastVertical", -1);
        rb.AddForce(-firePointDown.up * bulletForce, ForceMode2D.Impulse);
    }

    if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") < -0.01)
    {
        animator.SetTrigger("Shoot");
        GameObject bullet = Instantiate(bulletPrefab, firePointLeft.position, firePointLeft.rotation);
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
        bullet.GetComponent<Animator>().SetFloat("lastHorizontal", -1);
        rb.AddForce(-firePointLeft.right * bulletForce, ForceMode2D.Impulse);
    }

    if (gameObject.GetComponent<PlayerMovement>().animator.GetFloat("lastHorizontal") > 0.01)
    {
        animator.SetTrigger("Shoot");
        GameObject bullet = Instantiate(bulletPrefab, firePointRight.position, firePointRight.rotation);
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();
        bullet.GetComponent<Animator>().SetFloat("lastHorizontal", 1);
        rb.AddForce(firePointRight.right * bulletForce, ForceMode2D.Impulse);
    }
}
```
This will detect the direction the player is facing when the shooting is triggered and fire a bullet in that direction, while also playing the correct animation for the bullet so it faces the same way.

Save the Script and go back to the Editor. Click and drag each firePos in the Scene to the correct direction referenced in the Script on the player, and drag the bullet Prefab in the Assets Folder to the bulletPrefab in the Script as well. If you press Play, you should be able to press the button and the bullet flies out in the right direction.

### 8.
Create a new script called Bullet and add it to the bullet Prefab in the Folder, not any bullets in the Scene. Add these variables:

```
public Animator animator;
public Rigidbody2D rb;
public int damage;
```
Create an OnCollisionEnter2D function and add this:

```
void OnCollisionEnter2D(Collision2D collision)
{
    animator.SetTrigger("Hit");
    Destroy(gameObject, 0.1f);
    if (collision.gameObject.CompareTag("Destroyable"))
    {
        collision.gameObject.GetComponent<Enemy>().TakeDamage(damage);
    }
}
```
This will allow the bullet to be destroyed when it hits anything, and deal damage to an enemy if it hits one. Finally, add the following function:

```
    void OnBecameInvisible()
    {
        Destroy(gameObject);
    }
```
This simply destroys the bullet once it is off camera, so that it doesn't fill up the scene with bullets and cause the game to crash.

Save your Script and make sure to drag in the bullet's Rigidbody2D and Animator Components to the references in the Script, and change the damage amount to something other than 0. Press Play and you should be able to fire a bullet in whatever direction you are facing, with the player and bullet's Animations matching that, and when the bullet hits something or goes off camera it is destroyed, and it deals the specified damage to an enemy when it hits it.
