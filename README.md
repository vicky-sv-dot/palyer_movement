# palyer_movement
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using System;

public class Player : MonoBehaviour
{
    private Rigidbody myBody;
    [SerializeField] float v3Force = 3f;
    [SerializeField] float gravity = 0f;
    [SerializeField] private FixedJoystick joystick;
    [SerializeField] private GameObject JoystickObject;
    [SerializeField]  GameObject Cam;
    [SerializeField] PhotonView photonView;
    private CharacterController charController;
    private PlayerAnimations anim;

    void Awake()
    {
        if (photonView.isMine)
        {
            Cam.SetActive(true);
        }

        myBody = GetComponent<Rigidbody>();

        joystick = JoystickObject.GetComponent<FixedJoystick>();
        
        anim = GetComponent<PlayerAnimations>();
        
        charController = GetComponent<CharacterController>();

    }



    // Update is called once per frame
    void Update()
    {
        if (photonView.isMine)
        {

            myBody.velocity = new Vector3(joystick.Horizontal * v3Force * Time.deltaTime, myBody.velocity.y * Time.deltaTime,
                                                  joystick.Vertical * v3Force * Time.deltaTime);

            if (joystick.Horizontal != 0f || joystick.Vertical != 0f)
            {

                transform.rotation = Quaternion.LookRotation(myBody.velocity);

                anim.Walk(true);
                Move();

            }
            else
            {
                anim.Walk(false);

            }
        }

    }



    void Move()
    {

        if (joystick.Horizontal != 0f || joystick.Vertical != 0f)
        {

            Vector3 moveDirection = transform.forward;
            moveDirection.y -= gravity * Time.deltaTime;
            charController.Move(moveDirection * v3Force * Time.deltaTime);


        }
        else
        {
            charController.Move(Vector3.zero);
        }
    }
      
    public void Punch()
        {
        anim.Attack();
    }


    }



