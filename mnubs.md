---
layout: page
permalink: /portfolio/samples/mnubs
---

# Michael's Numerical Battle System (MNubs)

MNubs was a piece of code that handled a higher-or-lower style of combat for a cancelled prototype that I was designing and developing at Soft Boiled Elk. It's quite a simple script to be fair, but I think that it's easily extendable with the addition of seperate scripts for enemies and different weapons that do different damage. the WebGL example below shows off the script in action within Unity's runtime.

<iframe markdown="span" width="640" height="480" frameborder="no" border="0" overflow="hidden" src="/assets/mnubs/index.html"></iframe>

{% highlight csharp %}
/*
 * Michael's Numerical Battle System - M-NuBS
 * Date of creation - 13/12/17
 * code version - 1.0.0
 * Author: Michael Laws
 * Additional Authors: N/A
 */
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class MNuBS : MonoBehaviour {

    //Private variables for M-NuBS
    [SerializeField]
    private System.Random playerRand = new System.Random(3386); //Serialize private randoms to show in editor
    [SerializeField]
    private System.Random enemyRand = new System.Random(0294);

    //private User Interface references
    Text playerStats;
    Text enemyStats;
    Text playerNum;
    Text enemyNum;
    Text resultBox;

    //Private integers for random and statistics.
    //TODO: create structs for enemy stuff so I can just create boatloads of monsters and reference them at will
    int playerHealth;
    int playerDamage;
    int enemyHealth;
    int enemyDamage;

    private void Awake()
    {
        //References to private UI elements
        playerStats = GameObject.Find("playerStats").GetComponent<Text>();
        enemyStats = GameObject.Find("monsterStats").GetComponent<Text>();
        playerNum = GameObject.Find("yourNumber").GetComponent<Text>();
        enemyNum = GameObject.Find("enemyNumber").GetComponent<Text>();
        resultBox = GameObject.Find("resultText").GetComponent<Text>();

        //Values for integers above
        playerHealth = 35;
        enemyHealth = 50;
        playerDamage = 8;
        enemyDamage = 8;
    }

    private void Update() //Updated once per frame. Thisis a very inexpensive update so it can stay here for now. TODO: Create event instead
    {
        playerStats.text = "You:\nHealth: " + playerHealth.ToString() + "\nDamage: " + playerDamage.ToString();
        enemyStats.text = "Enemy:\nHealth: " + enemyHealth.ToString() + "\nDamage: " + enemyDamage.ToString();
    }

    public void Battle()
    {
        var playerResult = playerRand.Next(1, 20);
        playerNum.text = playerResult.ToString();                           //Make UI = data
        var enemyResult = enemyRand.Next(1, 20);                            //Player and enemy both roll a randomly seeded fair(ish) D20
        enemyNum.text = enemyResult.ToString();

        if (playerResult > enemyResult)
        {
            enemyHealth -= playerDamage;
            resultBox.text = "You win this round!";                         //If player rolls higher
        }
        else if (enemyResult > playerResult)                                //If enemy rolls higher
        {
            playerHealth -= enemyDamage;
            resultBox.text = "Troll wins this round!";
        }
        else if (playerResult == enemyResult)
        {
            resultBox.text = "Nobody won this round!";
        }

        //Death reset code
        if (playerHealth <= 0)
        {
            resultBox.text = "You have died";
            playerHealth = 35;
            enemyHealth = 50;
        }
        else if (enemyHealth <= 0)
        {
            resultBox.text = "You are victorious";
            playerHealth = 35;
            enemyHealth = 50;
        }
    }
}

{% endhighlight %}