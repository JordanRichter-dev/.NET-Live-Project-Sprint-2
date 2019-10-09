# .NET Live Project-Sprint 2

Personal Code Summary for the second team live project I was a part of at Prosper IT Consulting



## Introduction

During my time at The Tech Academy, I was assigned to 2 two-week sprints working on software to be used as a way to "manage a collection of construction jobs. Admins will be able to create and distribute a weekly schedule assigning users to certain jobs. Users will be able to keep track of which job they are assigned to for the week. There will also be the ability for admins to post company news and announcements, and chat functionality for the users to socialize."

My contributions to this second sprint of the project included adding back-end functionality.  I fixed a bug that allowed the admin to delete them selves, I enabled the app to capture from the database which users were suspended, I created a partial view that allows users to be separated by "active" or "suspended", I enabled a Job object to be deleted along with all of it's attached objects from different tables of the database, and I added a list of all users to the admin Dashboard view.

Listed below are the stories I worked on, a brief description of their expectations, and the code I created to complete them.

## User Stories

- [Remove Ability To Suspend and Change Role for Self](#remove-ability-to-suspend-and-change-role-for-self)
- [Capture Suspended](#capture-suspended)
- [Separate Users by Type](#separate-users-by-type)
- [Deleting a Job with attached objects](#deleting-a-job-with-attached-objects)
- [User List in Dashboard Admin View](#user-list-in-dashboard-admin-view)





## Remove Ability To Suspend and Change Role for Self

When the user is logged in as an admin, they have the ability to suspend or change the role for any user. We don't want the admin to accidentally change their own role. Add in a check in, with an error alert, to make sure the user isn't changing their own status. Test that this works (Create a dummy admin account so you don't accidentally log yourself out of admin abilities!). Once it works, make sure the drop down for user role doesn't display for the logged in user.



Here is the original page:

![img](images/AMwEvrHh0gQGCD0MxDs-JgnNLBASdE6uiTXcZYbZ-QIhADUskK7b46HUDhkJRq9_IQVh3oCl3_yvSSLWh6NumnLWj9jHMIhBl55tvsed2JBg_24cGGjk08paf2Pklr164xHbAepM-20191009125326624.png)



Add lines 40 - 43 to UserController.cs to prevent user from changing own role:

![img](images/hubjUhyppShwCvhMP1RjE0KvkP5N1OLNeu9Vk_Li9hQL8RWU7RAeoopu5TGjKCXoJNGxMuzSI7CCwi3dYYDGRfnOMA-H3CHz-D_bNm5kEsmROYyZFbSQRUqbJVwgOap8Azegdszo.png)



This throws a user friendly error message if the user tries to change their role from "admin" to anything else.  It then refreshes the screen to re-display current user role as "admin" instead of whatever user tried to change it to.  This is accomplished by calling the Json "message" as added in lines 209 - 212 of Site.js:



![img](images/cPCq4AwtTOkZKbCQap7-o98Ty9jay2pSVakfbYhcSjgTyX0ViinGxayUfBhiPtRJ2qFLB6JXzfa_siK9N63x9qpy9I7UVnBGmUvH8QGj0eIW37YWcbZuGKQyI2cLuXsSAU55d9aX.png)



This removes functionality from the dropdown Role change menu, but the User Story asks for the removal of the buttons altogether for the active user.  This is accomplished by adding an if loop to the code that displays the dropdown in the _UserList.cshtml partial view file as seen on line 49:

![img](images/5x9UdKLv7rYG2nNIV7cfduGYdz9DVUH1im9OnW4C-qTRjj8JRcnGhSEsYrFAv-vzQpoMitytsUKS34oe-3tkM98-U-SM5OUdA-2INjMPEAdFXiwejYm2Gl_Y_htAL5Ky45LOIy4O.png)



I add this to line 62 in order to get rid of the "Remove User" button from the active user's display, as well.

Here is the finished page (I am logged in as NewAdmin):

![img](images/ZV2iZFyMYt3nHjHHH3NaqjQ_Ulx_2XRjffdI3NXJKfwB14VMyXEY-G1oYy6g96orHlR6kmjghebM7rPeZDlZcnNQlDrV_Bc9hRhp8bnFSDZwb3WnKDysWmRNjHJLAFEdueeIIUtQ.png)







### Capture Suspended
There is a checkbox for the suspended bool attribute in the user class, which doesn't actually bind to the property and change the user's status. Have the checkbox actually change the property value, and also have it display properly for the existing value.


In the UserController, I added this bit of code for updating the database to mark a user as "suspended" or not suspended:



![img](images/9OXjvl-oCCj7y-ndlJX6mQRPDTh8jkzDJW4QdVd0gIIGB1bRJ0uWX4sNklAAGbjZv16o2WyvPCpabZOxxLBVHyCqdHG6Y6Ah3i356JE6YF5sqKzgIXbMiQ9AQJM7g_u7XvKYUzTE.png)



I then modified and added code in the _UserList.cshtml partial View to call the Action "SuspendUser":

![img](images/oA0Aq-6jciAUOAByAE9xibXqMTcbNm5HiMVUh4gbdpquDIKpe-z5KY5TXe2nNMgTXLkjKVwI-vCVg9ILKVwqgyEvJVY8WFi_VceC1n05OkE1R06QxtH8L2QEd9zhtxD1f4zIUTSc.png)



Finally, I added an Ajax submit form in the Site.js to display warnings, confirmations, and to reload the page with the correct suspended status of each user.



![img](images/4Ap5CNizoypOmH02iWvJfPEzNpakdFosF_1aan1YPc0QuH63uddmeSE8DwDOhQjQn8FlsRdxfVkSWewMuI127Sxnk4XRP6ZYcXzzjR_9RnstsNhMQa8IX2j_0QFpRDb4bmzjRJCX.png)



### Separate Users by Type

 Now that we have added the ability to suspend a user, we want to separate them into a different list for the admin. Create a new shared partial view that lists only Suspended users. Filter the old Master User List Partial to not show Suspended Users. Render the partial view in the All Users list.Then add a Link to the Dashboard Users section "View All Users" that will have the Suspended Partial View Display below the Active Users List.    



This is the original  User List in the Admin Dashboard/Home page before:

![img](images/kHceHjkC6PuUFzx5kG4MaO2ivp2wwsDbPlpGzYAF8GdNIZIEN0CHTCyQISSsOW9MFDThlfrsLZuUZTjdAKqS9aufA0mCtsnA8-trJQpH8J3g8HQFODfZGOUtCfmZyj2t3zdhQ-hX.png)





...and the nav bar:



![img](images/jenvAD78fICznw5rz79q3tdV6r2GKILD8jnPWOiMPKvY_Ozcn7nUIsvNKt1xtRJOW0QFUjqoQhEeiKBSJfHrokwFwgdmV_Mi70gx-CJT55uToBkUiT1KkGJVGJimUECkgduy-ZMa.png)





I start by creating a  _SuspendedUsers partial view that only displays suspended users:

![img](images/CyvO2EFSpr3M9xjIxgeVrel-MZ3MSg9NvWOnZVqMZDp4gFsRUOQmvshMYFQXb5eaXZEi4XvYWv_kRF-V-G3sUbt09XOtuLQsjf9TXO9RP0lKX_k82HDN8UXK-ygXuVdSsKA3xQOk.png)![img](images/sqGehSonlmLMNll_nPNRKHq08VeLCRNIeDa4TkfIUjBnxH99a8uTePNq3ZXqNaqwiOtmPnJC8nY6WQ9S8h_mg-kLRWXvyeTX9k0yS32ZVPZmyV0VRQtcV9P3GQdQMiZa1x1H3vlv.png)![img](images/obe_f2V-EukINpZVyp-o_OhYxXFsk8jOmmeCN7XP54rHKYSzS245Ijq7XMhbC186AoBpRChlL2XEo_smoMWNFSoOrH8rJB1wpyKy-bOtbj8vALkfaWHiOywqGFeL2eA3di9ExyN_.png)







In the Dashboard.cshtml, which is the home page for the Admin, I add a RenderAction of _SuspendedUsers partial:



![img](images/InInGmSzl7nhcaSEIqRL8nCPfZfTuRKiR8Vt2Yb7Xq7hoBM6bsCkdMTNzwOVM7AvSVgiNJpl2o_ctWKTdxVrkrwXRGJSiGcKL1XFp0GWwPn-ySYwaLatd6HSSGaxvRO1l2CPaWLN.png)



In the _UserList.cshtml partial view, I add an "if" loop that only displays Users that are not Suspended:

![img](images/mCm1A6XBTXdJ5vfQ70XswylrHXLEYo8itW_T-gToEosTmxygRBfvphyNBMKqnAuDepxCwBlH9eFT7amwRO_ADsjvjpLDlh9zFAfSfC1r2R3jOV_u0g4B4FbDHqgHKVwpGmXlAs69.png)



I add "location.reload();" to the Site.js that reloads the page when users are suspended or re-activated:

![img](images/vQLWmldVW6A1ELMlU4VJIidRJN4olNBYM-uoPzoAHbyXiFXRaoKX-tK0SCs80zsqlkvvU0lmSoo8BNuDh4_ke8adcLYfz272bqMgWzgo2R9XsMBCbdKP-XVBiwSCB0ImasJxbK7w.png)



The User List in the Admin home page/Dashboard now looks like this:

![img](images/saAhNX4pwX2dMPfujJOsJ5lyqNKQNSlDhsYwHoygKBGZ2tsg74f-zFy4Lsm2XnReyPVvvmeXk3bTu4zQiZSPMUQ2usb8iDaasNDPBgTZHkkODdYZao8a1v-PJzbuBHWDZBNda1Ow.png)



I add an Index View for the User that uses the Index ActionResult in the UsersController:

![img](images/bYYKSeCSYKsQuK0m5wa0Z87BpJL7eIB5n-33yMqGWBeFvb-Zwpt5PohXeK-rCihdozyXWWGSV1FxsQtd5RWYrZDArySCULD46HWlGD9Il_6ZUqpPUgvP0E0JKhJd0UOAqHF6MC4D.png)



Then, in the _Layout.cshtml Partial View, I added the ActionLink reference to the Nav Bar:

![img](images/LDt9lAV9-OKte3TSYDeP6ESAiyzaH8A18vo49UZ8MuTH2FNRhCZySt3uFFpwXEsJgcSeq_SzWiQcefSbUeAfJcDx2q9qZDfJ52OTpuuTRNoCSvnzrd75tPGTfK76PBplmWZbvV7O.png)



...displaying this:

![img](images/6Kcbai8I9nvgFMHBmpnIv5T8po1P8Q7680J8IOJmxg_ejT4psyAQD08VPpg6OKgEy5_THXw_cjNioP9m9Nfw6a7JwnE2cBA-X5CsjIf8k3T8QYCGTeU13AUURJzhtLUkH0ObxneM.png)



The "All Users" link in the nav bar then displays the Users "Index" view:

![img](images/44V_u7HUM3j7D7AXVVWpxmNRAcPQkwxuWEJ5ORHZpd2ZIuM_Soq4W3IfUhL3Y1hbC4plYlo0yjDE4ew28m1aTqcNmd5eJRX2Y_Q2lySk1YodJnK4nw7dw4tkdIpbGcLSoM6cGfW6.png)







### Deleting a Job with attached objects
When trying to delete a Job item which is associated with a Schedule item or other items, the application crashes due to database related error.  We need to prevent this crash by creating a "User-Friendly Error Message" and error-handling in the back end. This handling will need to separate out items that may mean the user doesn't want to delete the job, and also make the necessary changes in order to be able to delete both the Job and its associated items successively. 
1) Using dummy jobs, implement the ability to delete a job with all fields filled out or attached (Shift Time, JobSite, JobOther, Manager, etc) - note that JobSite and Manager should not be deleted from the database, but ensure you can delete a job if those are included.
2) Create a pop-up message that tells the user there are still future schedules associated with the job if this is the case, and ask if they want to continue-- include the ability to cancel the request, and this may require sending a bool for future schedules into the ViewBag for client side verification.
3) Add the ability to delete all schedule items associated with the Job if the user wants to delete a job anyway.




The app crashes when user attempts to delete a Job.  Here is the original code in the DeleteConfirmed function in the JobController:

![img](images/WzX4YLnR5En7haqedwzvBiM2Wm9GHrmk4YH0Ad8RvpurxVbSNygG81APqUD3t0_V4tmoSpYGpde2peAych2pTwlgORXlKR63m2d4Q7RuiV8UJ1CNQyloP4m6www-njIItDplgaCf.png)



Here is the new code DeleteConfirmed that allows for objects associated with the Job being deleted to also be removed from their respective databases also:  

![img](images/-ZRUyBlfRFbAd7nPHhHmIuxjMVaoTaasJpchKsuBvJYGmAqNN3ASHQIIkINIal1d5qCyRimJobyMX1cydlC0QrmRQ-6vdvg_nSO2SbiQwyakpD9LcqH8bQU3oDjNng8g8wtuUjey.png)



In order to add a confirmation warning to pop up when a Job is already Scheduled, I add this Javascript function:



![img](images/d6IjcAW9eE_bPF73fu4kLr0sxYMUTGQPvIQPj9JuI59VjgnH5c8uh7O5m3Erh17bg7zCucMwdcwQVny1clMydzZ-IGveshXA9J2PWB2lr7mssaQtdOoL4X_B465PSaQcYxNdyF_X.png)



If the user presses "OK", the DeleteConfirmed CS function in the JobsController is run, if the user presses "Cancel" the warning box closes.

In order for that warning to only appear when there are one or more Schedules associated with the Job being deleted, the Delete.htmlcs View file is changed from this:

![img](images/MgJTx-D3ljfa7EY_gzFoTQAlQKONCmVQZNJzC545lUwPYwbUOe2xsQvJ5RYDkQ_1B2-B-4K9b3E3Viiuz5jLWvqiIFkelYyZnI3_KFOnfZ6mLelOSVc34NZl4zlSlO-7TpqzCtHx.png)



To this:

![img](images/5WbNX754u_kaGYZcoMQT3JVlAfzFQGTSlXJ96adJR3OxOVcLV0cEKBDEq_vuFq_kBCLq6NHzB_1YUnuVBq5LMemRg0KDl6ZKNcOhZK5ND_1S0z8qKmW3CfRg6aSYJ91KgrrXAvWT.png)



The IF loop counts the number of Schedules and proceeds accordingly.  I also now display the name of the Job to be deleted as an extra precaution/clarification.

Here is what the user sees:



![img](images/Adczz7ePzyZPnmANcHl24WKdQzc0UDtNhdKUnEPGDtditH0aRSB-oieJhgAONwiNXCV0-zxnxS2mhrE2CRtK53TagfUflZv8XmTe2-0f_kq11JBid3p5HlP0ZwN5EFgYsUvVBaAZ.png)



There are two Dummy Jobs.  One has no Schedules, Two has one Schedule attached.  Here's the Schedule view for reference:

![img](images/yFadEdBZwIbqULNu7TbggtP35fYBhOQ-_U8t73_wq0C44AbokfMFGuX3nusBe3EUAqkTPCHsjyKrsD4X7oQBNqsfMd0IB1vl6F7bvIz_ekkL5jVwtWK8kGJdnsID8FQ_--G0KBwO.png)



When the user goes to delete "Dummy Job 1" with no schedules attached, they are allowed to delete from this page with no warnings after pressing the "Delete Dummy Job 1" button:

![img](images/VfTxAvBlsVcpuiS0WEl31_W6g0TzuyKoyu2_r7mjNEU1WHEFgemPR6DcXe7uR0H4080Ie0shnm7Zm1wj630nND7F0cJn-NVO1humdTbc6Ddzf5JdOxtefjdtvvR0JcbpyQDSuid4.png)



When the user goes to delete "Dummy Job 2" with one schedule attached, the following warning appears after pressing "Delete Dummy Job 2":



![img](images/-6IyXW0O4ozX2LOglVaXF7yJq-WM0DEQGdC16xZptZmwbAKrOuIaf0-A-TbYWiQ0YkFRvLe3B1DNmO__UwQt3ZOIDV9p32wv1-BMsyxNtO6hFSWlZ8-jzR0bGqg9I38uHHG-T8cl.png)



Pressing "OK" stays on the current page, while "OK" runs the DeleteConfirmed Controller function.  

Here is the Job list with the two Dummy Jobs deleted:

![img](images/LFrwEDyk3kD41IOAvAJErMNj8jHSPRr80lOg-tDC02Wp_kKaspdK1CStLG_UZ1bcHY-1q2yRKPdurTucR46UXnJnJ8waEir4VN-8jCt4YpfeDc2zHsr9zaW7B5zG2T9u1WNEkdaY.png)





### User List in Dashboard Admin View

The admin is able to see all User (Active, suspended, and unregistered) by just clicking the " All Users"  navigation item which takes him (Users/Index view page). The Admin is also able to see this list from the Admin Dashboard (Home/Dashboard view page) under User List.  But the "Unregistered Users" are not displayed here. Your job is to fix this bug. 

Before:

![img](images/gBL0Gv8Ic3YEYB3R-WXhkcfDc1l-tt0XpY135IwcojBAzddptnUGDWcYg1Y1FfI2foLlJgJF57_XVZlw4OPUTYCZy-drbVnh77khNw737o4P4gfWPmbPk8vTwI-312Ne8_-ukwVh.png)

This is a very simple story, especially since I already implemented the User/Index view page previously.   Here is the existing code for the Home/Dashboard view page:

![img](images/L2354TpI1gMuPvB4GU7QuvcMC1w5A1NjpmRqX9F9ImBvh3YtDgZAn_Kui-_br22m98CTu6UXEgPhaSvGXCcyxhp9uRX7seJp8wQcAD_izoiLyGEX0LWsbK2H8HyaAmwgNjc7z-Cf.png)



I add line 52 though 54 to render the _UnregisteredUsers partial view as I did in User/Index:

![img](images/EqW2aCEVSRu39JUURXRGmrjD00yUhR9jJ5ZXqISKKvyFrgCk3JxR4q4f5H9auBUjw10khFAQiLmmGYQPoLooG22PoLTlvIduF9YFHevPV_JVeCXhdDR0i7JOsQ4Ea5B-KPSaBYUe.png)



And that's it.  The Dashboard page now shows Registered, Suspended, and Unregistered Users in their own categories:



![img](images/b7m_tng3U6H53KXs9XWaX0KsoLy5lm14yxJyXrT42MIRc_DGAk-5b8qP0AyDiLSMZwDRvTdq4LMIbu-_j3D_rdHnMQJfEYg6RSZkMIqs0DXUgGvJ2jNqTO31uH6yO501dHFdoJjn-20191009130553919.png)



