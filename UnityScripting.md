<div align="center"> 




<img src="https://www.citypng.com/public/uploads/small/11663347550abmzciym29utfu50oxy7gqzjndph5jybsz0swz6vxcckt6vrpv7bazqg1pep0jq3rkhnsh0a7hkihprlicy15ggtvcppvdfn9hbx.png" width="200px" style="padding-bottom: 20px;">
<img src="https://upload.wikimedia.org/wikipedia/commons/4/4f/Csharp_Logo.png" width="140px">
</div>

---


<div align='center'>

## 📚 Table of Contents 📚



---
 [Coroutines](#coroutines)
 <br/>
 [Vectors](#vectors)
 <br/>
 [Camera](#cameras)
 <br/>
 [Misc](#misc)
<br/>
[Mouse](#mouse)

</div>

---
<div align='center'>

## ⏱ Coroutines (aka Timers) ⏱️ <a  id="coroutines"></a>

</div>

---

- `StartCoroutine( functionName() )`
- `yield return new WaitForSeconds( seconds )`
- place clean up code after `yield return new`
```cs
   private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("Powerup"))
        {
            Destroy(other.gameObject);
            hasPowerup = true;
            powerupIndicator.SetActive(true);
            StartCoroutine(PowerupCooldown()); // Starting the countdown
        }
    }

    // Coroutine to count down powerup duration
    IEnumerator PowerupCooldown()
    {
        yield return new WaitForSeconds(powerUpDuration); // anything after this yield return is executed 
        hasPowerup = false;                               // when the WaitForSeconds() is up.
        powerupIndicator.SetActive(false);
    }
```
<div align='center'>

---
## 📉 Vectors 📉 <a id="vectors"></a>
---

</div>

### adding forces directionally

>` Vector3 awayFromPlayer = other.gameObject.transform.position - transform.position;`

```cs
private void launchMissles()
{
    Enemy[] enemies = FindObjectsOfType<Enemy>();
    foreach(var enemy in enemies)
    {
        Vector3 initialLocation = Vector3.Lerp(gameObject.transform.position, enemy.transform.position, 1 / Vector3.Distance(gameObject.transform.position, enemy.transform.position));
        var msl = Instantiate(missle,initialLocation, missle.transform.rotation);
        msl.transform.LookAt(enemy.transform);  // Add this line
        Vector3 forceDirection = (enemy.transform.position - transform.position).normalized;
        msl.GetComponent<Rigidbody>().AddForce(forceDirection*80, ForceMode.Impulse);
    }
}
```
> Use .normalized for adding forces towards an object in the distance, without it scaling based on how far it is.
```cs
    void Update()
    {
        // Set enemy direction towards player goal and move there
        Vector3 lookDirection = (playerGoal.transform.position - transform.position).normalized;
        enemyRb.AddForce(lookDirection * speed * Time.deltaTime);
    }
```
---

<div align='center'>

## 📽️ Camera Rotation ️️📽️ <a id="cameras"></a>

</div>

---
- Camera is a `child` of an arbitrary `Focal Point` game object, which is an empty object.
- The script below is a component of and attached to ` Focal Point `
- Focal point has a rigidbody with its `X, Y, Z` rotations frozen.
```cs
    void Update()
    {
        float horizontalInput = Input.GetAxis("Horizontal");
        transform.Rotate(Vector3.up, horizontalInput * speed * Time.deltaTime);

        transform.position = player.transform.position; // Move focal point with player

    }
```

---

<div align='center' id="mouse">

## 🖱️Mouse Interactivity🖱️

</div>

---

### Getting a point in world space with mouse click

>https://gamedevbeginner.com/how-to-convert-the-mouse-position-to-world-space-in-unity-2d-3d/#screen_to_world_3d


---

<div align='center'>

## 🪢 Miscellaneous 🪢 <a id="misc"></a>

</div>

---

1. `GetComponent<T>()`
>Used to grab a component from a game object utilizing \<T\> as an object type.
2. _`public`_ vs _`private`_
>public variables are accessible within the Unity editor, and are available to other scripts -- whereas private variables are not.
3. finding multiple objects uitilizing `FindObjectsOfType<T>()`
```cs
Enemy[] enemies = FindObjectsOfType<Enemy>();
        foreach(var enemy in enemies)
        {
            Instantiate(missle);
        }
```

<br/>

