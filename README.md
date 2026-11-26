# AWS IAM Users, Groups & Permissions Lab

I just finished this hands-on AWS Identity and Access Management (IAM) simulation project where I set up IAM user groups, attached least-privilege permissions policies, created IAM users, and tested each user's real console access in an Incognito browser session.

📂 **All project screenshots:** [Browse the `screenshots/` folder](./screenshots/)

---

## Table of Contents

- [Simulation Scenario](#simulation-scenario)
- [Task 1: Exploring the IAM Dashboard & Policies](#task-1-exploring-the-iam-dashboard--policies)
- [Task 2: Creating User Groups with Permissions](#task-2-creating-user-groups-with-permissions)
- [Task 3: Creating IAM Users](#task-3-creating-iam-users)
- [Task 4: Using the User Group to Add Users](#task-4-using-the-user-group-to-add-users)
- [Task 5: Testing IAM User Permissions in Incognito](#task-5-testing-iam-user-permissions-in-incognito)
  - [5.1 Preparing the Sign-In URL & Incognito Session](#51-preparing-the-sign-in-url--incognito-session)
  - [5.2 Testing `user-1` (`S3-Support`)](#52-testing-user-1-s3-support)
  - [5.3 Testing `user-2` (`EC2-Support`)](#53-testing-user-2-ec2-support)
  - [5.4 Testing `user-3` (`EC2-Admin`)](#54-testing-user-3-ec2-admin)
- [Task 6: Updating Group Membership & Multi-Group Access (`user-3` + `S3-Support`)](#task-6-updating-group-membership--multi-group-access-user-3--s3-support)
- [Key Takeaways](#key-takeaways)
- [Screenshots Folder & Full Index](#screenshots-folder--full-index)

---

## Simulation Scenario

For this simulation, I configured IAM users and groups to support the following business scenario:

> Your company is growing its use of AWS services, and is using many Amazon Elastic Compute Cloud (Amazon EC2) instances and Amazon Simple Storage Service (Amazon S3) buckets. You hire three new employees and want to give access to new staff, depending on their job function, as indicated in the following table.

| User | In Group | Permissions |
| :--- | :--- | :--- |
| `user-1` | `S3-Support` | Read-only access to Amazon S3 |
| `user-2` | `EC2-Support` | Read-only access to Amazon EC2 |
| `user-3` | `EC2-Admin` | View, start, and stop Amazon EC2 instances |

![Simulation Scenario](./screenshots/00-simulation-scenario.png)

---

## Task 1: Exploring the IAM Dashboard & Policies

1. I opened the **AWS Management Console** and navigated to **IAM (Identity and Access Management)**.
2. On the **IAM Dashboard**, I checked the initial account state (`0` user groups, `0` users, `66` roles, `26` policies, `0` identity providers) and noted the **Sign-in URL for IAM users in this account**.
3. Next, I opened **Access management -> Policies** to inspect the existing AWS-managed and customer-managed policies available in the account (over `1,227` policies total).

![IAM Dashboard Initial State](./screenshots/01-iam-dashboard-initial.png)
![IAM Policies List](./screenshots/02-iam-policies-list.png)

---

## Task 2: Creating User Groups with Permissions

To follow AWS best practices, I attached permission policies to **User groups** rather than assigning policies directly to individual users.

1. **Create the `EC2-Admin` group:**
   - Navigated to **Access management -> User groups -> Create group**.
   - Entered the group name `EC2-Admin` (after initially typing `Ec2-admin`, I corrected the casing to match the scenario specification).
   - Under **Attach permissions policies**, searched for and selected the customer-managed policy **`EC2-Admin-Policy`**, then clicked **Create user group**.

   ![Create EC2-Admin Group](./screenshots/04-create-group-ec2-admin-policy.png)

2. **Create the `EC2-Support` group:**
   - Clicked **Create group** and named it `EC2-Support`.
   - Filtered and attached the AWS-managed policy **`AmazonEC2ReadOnlyAccess`**, then created the group.

   ![Create EC2-Support Group](./screenshots/05-create-group-ec2-support-policy.png)
   ![EC2-Support Group Created](./screenshots/06-ec2-support-group-created.png)

3. **Create the `S3-Support` group:**
   - Clicked **Create group** and named it `S3-Support`.
   - Filtered and attached the AWS-managed policy **`AmazonS3ReadOnlyAccess`**, then clicked **Create user group**.

   ![Create S3-Support Group](./screenshots/07-create-group-s3-support-policy.png)

4. Verified all three groups (`EC2-Admin`, `EC2-Support`, `S3-Support`) appeared with **Defined** permissions, completing **Task 2: Creating user groups with permissions**.

![Task 2 Complete - User Groups Created](./screenshots/08-task2-complete-user-groups-created.png)

---

## Task 3: Creating IAM Users

Next, I navigated to **Access management -> Users** (starting with `0` users) and created the three new employee accounts.

![IAM Users Empty List](./screenshots/09-iam-users-empty.png)

1. **Create `user-1` (assigned to `S3-Support` during creation):**
   - Clicked **Create user** and entered `user-1`.
   - Selected **Provide user access to the AWS Management Console** and chose **I want to create an IAM user**.
   - Selected **Custom password**, entered `Sim-Password1`, and unchecked **Users must create a new password at next sign-in** so I could test logging in directly during the lab.
   - On **Set permissions**, chose **Add user to group** and selected **`S3-Support`** (`AmazonS3ReadOnlyAccess`).
   - Reviewed and created the user, then returned to the users list.

   ![Specify user-1 Details](./screenshots/10-create-user1-details.png)
   ![user-1 Custom Password](./screenshots/12-create-user1-password-reset-unchecked.png)
   ![Assign user-1 to S3-Support](./screenshots/13-create-user1-assign-s3-support.png)
   ![user-1 Created Successfully](./screenshots/14-user1-created-successfully.png)

2. **Create `user-2` (assigned to `EC2-Support` during creation):**
   - Clicked **Create user**, set username to `user-2`, enabled console access as an IAM user, and set custom password `Sim-Password2`.
   - Added `user-2` to **`EC2-Support`** (`AmazonEC2ReadOnlyAccess`).
   - Confirmed details on **Review and create**, clicked **Create user**, and continued past the retrieve password prompt back to the users list.

   ![user-2 Custom Password](./screenshots/17-create-user2-custom-password.png)
   ![Review and Create user-2](./screenshots/19-create-user2-review-ec2-support.png)
   ![Users List with user-1 and user-2](./screenshots/21-users-list-user1-user2.png)

3. **Create `user-3` (created first without a group, to be added via the group page in Task 4):**
   - Clicked **Create user**, set username to `user-3`, enabled console access as an IAM user, and set custom password `Sim-Password3`.
   - Continued through **Set permissions** without selecting a group yet, reviewed, and clicked **Create user**.

   ![user-3 Custom Password](./screenshots/22-create-user3-custom-password.png)
   ![user-3 Set Permissions](./screenshots/23-create-user3-set-permissions.png)
   ![user-3 Created Successfully](./screenshots/24-user3-created-successfully.png)

---

## Task 4: Using the User Group to Add Users

Before adding `user-3` to `EC2-Admin`, I also inspected how group-inherited permissions look on a user and inside the policy document:

1. Opened `user-2` -> **Permissions** tab and verified **`AmazonEC2ReadOnlyAccess`** was attached via **Group `EC2-Support`**.
2. Clicked **`AmazonEC2ReadOnlyAccess`** to review its **Summary** (showing read/list access for CloudWatch, EC2, EC2 Auto Scaling, ELB) and its **JSON** policy (`ec2:Describe*`, `elasticloadbalancing:Describe*`, `cloudwatch:ListMetrics`, `cloudwatch:GetMetricStatistics`, `cloudwatch:Describe*`).

   ![user-2 Permissions Tab](./screenshots/25-user2-permissions-ec2-support.png)
   ![AmazonEC2ReadOnlyAccess Summary](./screenshots/26-policy-ec2-readonly-summary.png)
   ![AmazonEC2ReadOnlyAccess JSON](./screenshots/27-policy-ec2-readonly-json.png)

3. Navigated to **User groups -> `EC2-Admin`** (`0` users) and clicked **Add users**.
4. Selected **`user-3`** (which had `0` groups) and clicked **Add users**.
5. Verified `user-3` was added to `EC2-Admin` and confirmed under **Users** that `user-1`, `user-2`, and `user-3` each belonged to `1` group with console access **Enabled**, completing **Task 4: Using the user group to add users**.

   ![EC2-Admin Group Details](./screenshots/28-ec2-admin-group-details.png)
   ![Select user-3 for EC2-Admin](./screenshots/30-ec2-admin-select-user3.png)
   ![Task 4 Complete - All Users Assigned](./screenshots/32-task4-complete-all-users-assigned.png)

---

## Task 5: Testing IAM User Permissions in Incognito

### 5.1 Preparing the Sign-In URL & Incognito Session

1. Returned to the **IAM Dashboard** (`3` user groups, `3` users, `66` roles, `27` policies) and copied the **Sign-in URL for IAM users in this account**.
2. Opened a **New Incognito window** in Chrome so my admin session wouldn't conflict with the IAM user tests, pasted the sign-in URL, and loaded the **Sign in as IAM user** page.

![IAM Dashboard Summary](./screenshots/33-iam-dashboard-summary.png)
![Sign-in URL Copied](./screenshots/34-iam-signin-url-copied.png)
![Incognito Sign-in Page](./screenshots/37-iam-signin-page.png)

---

### 5.2 Testing `user-1` (`S3-Support`)

- **Expected behavior:** Read-only access to Amazon S3 (`AmazonS3ReadOnlyAccess`). Can view buckets and objects, cannot upload or modify objects, and has no access to restricted services like Cost & Usage or Applications.

1. Signed in with `user-1` / `Sim-Password1`.
2. On **Console Home**, confirmed unrelated widgets (Applications, Cost and usage) showed **Access denied** as expected.
3. Opened **Amazon S3 -> Buckets** and verified `user-1` could list buckets and open `sim-website` to view its objects (`index.html`, `new-report.png`, `script.js`, `style.css`).
4. Tested write restrictions by clicking **Upload**, selecting `index.html` from my Downloads folder, and clicking **Upload** to `s3://sim-website`.
5. Confirmed **Upload failed** with error **`Access Denied`**, proving `user-1` strictly has read-only S3 access.

![Sign in as user-1](./screenshots/39-signin-as-user1.png)
![user-1 S3 sim-website Objects](./screenshots/42-user1-s3-sim-website-objects.png)
![user-1 S3 Upload Attempt](./screenshots/44-user1-s3-attempt-upload.png)
![user-1 S3 Upload Access Denied](./screenshots/45-user1-s3-upload-access-denied.png)

---

### 5.3 Testing `user-2` (`EC2-Support`)

- **Expected behavior:** Read-only access to Amazon EC2 (`AmazonEC2ReadOnlyAccess`). Can view EC2 instances, cannot stop/start/modify them.

1. Signed out of `user-1` and signed in as `user-2` / `Sim-Password2`.
2. Navigated to **EC2 -> Instances** and confirmed `user-2` could view both running `t2.micro` instances (`Application server` and `Web server`).
3. Selected `Application server` (`i-0aeba3d612be4e6ec`), opened **Instance state -> Stop instance**, and tested the action.
4. Confirmed AWS blocked the request with **`Failed to stop the instance i-0aeba3d612be4e6ec`** (*"You are not authorized to perform this operation... not authorized to perform: ec2:StopInstances"*).

![Sign in as user-2](./screenshots/46-signin-as-user2.png)
![user-2 View EC2 Instances](./screenshots/48-user2-ec2-instances-view.png)
![user-2 Attempt Stop Instance](./screenshots/49-user2-ec2-attempt-stop-instance.png)
![user-2 EC2 Stop Unauthorized](./screenshots/50-user2-ec2-stop-unauthorized.png)

---

### 5.4 Testing `user-3` (`EC2-Admin`)

- **Expected behavior:** Can view, start, and stop Amazon EC2 instances (`EC2-Admin-Policy`), with no S3 bucket permissions initially.

1. Signed out of `user-2` and signed in as `user-3` / `Sim-Password3`.
2. Opened **EC2 Dashboard -> Instances**, selected `Application server` (`i-0aeba3d612be4e6ec`), and clicked **Instance state -> Stop instance**.
3. Confirmed **`Successfully stopped i-0aeba3d612be4e6ec`** (instance state transitioned to `Stopping`).
4. Also navigated to **Amazon S3** as `user-3` to verify `user-3` did not yet have access to view the S3 buckets.

![Sign in as user-3](./screenshots/51-signin-as-user3.png)
![user-3 Stop EC2 Instance](./screenshots/53-user3-ec2-stop-instance.png)
![user-3 EC2 Instance Stopped](./screenshots/54-user3-ec2-instance-stopped-success.png)
![user-3 S3 No Access Initially](./screenshots/55-user3-s3-no-access-initially.png)

---

## Task 6: Updating Group Membership & Multi-Group Access (`user-3` + `S3-Support`)

To verify how IAM group changes take effect and how a user can belong to multiple groups:

1. Switched back to my admin browser session on the **IAM Dashboard**.
2. Opened **User groups -> `S3-Support`** (which currently only contained `user-1`) and clicked **Add users**.
3. Added **`user-3`** to `S3-Support`. Confirmed `S3-Support` now contained both `user-1` (`1` group) and `user-3` (`2` groups: `EC2-Admin` + `S3-Support`).
4. Returned to the Incognito session signed in as `user-3` and opened **Amazon S3 -> Buckets**.
5. Confirmed `user-3` could now immediately see all S3 buckets (including `sim-website`), proving permissions update dynamically through group membership, then signed out to finish the lab.

![S3-Support Group Before Adding user-3](./screenshots/57-s3-support-group-details.png)
![S3-Support After Adding user-3](./screenshots/59-s3-support-user3-added.png)
![user-3 S3 Buckets Now Accessible](./screenshots/60-user3-s3-buckets-now-accessible.png)
![user-3 Sign Out](./screenshots/61-user3-console-signout.png)

---

## Key Takeaways

- **Principle of Least Privilege:** Every user only got the exact permissions needed for their job role—read-only S3 for `user-1`, read-only EC2 for `user-2`, and EC2 view/start/stop for `user-3`.
- **Group-Based Access Control (RBAC):** Attaching policies to IAM user groups (`S3-Support`, `EC2-Support`, `EC2-Admin`) instead of individual users makes onboarding, auditing, and updating permissions clean and scalable.
- **Explicit Verification:** Testing each account in a separate Incognito session confirmed both allowed actions (listing S3 objects, stopping EC2 instances) and denied actions (`Access Denied` on S3 upload, `Unauthorized` on `ec2:StopInstances`).
- **Dynamic Multi-Group Permissions:** Adding `user-3` to `S3-Support` granted S3 read access right away without modifying any user-level inline policies.

---

## Screenshots Folder & Full Index

All 62 screenshots from this project are stored in the [`screenshots/`](./screenshots/) folder:

🔗 **[Open `screenshots/` Folder](./screenshots/)**

<details>
<summary>Click to view the complete list of all 62 screenshots in chronological order</summary>

| # | Screenshot | Description |
| :-: | :--- | :--- |
| 00 | [00-simulation-scenario.png](./screenshots/00-simulation-scenario.png) | Simulation scenario table (`user-1`, `user-2`, `user-3`, groups & permissions) |
| 01 | [01-iam-dashboard-initial.png](./screenshots/01-iam-dashboard-initial.png) | Initial IAM Dashboard (`0` groups, `0` users) |
| 02 | [02-iam-policies-list.png](./screenshots/02-iam-policies-list.png) | IAM Policies list (`1227` policies) |
| 03 | [03-create-group-ec2-admin-name.png](./screenshots/03-create-group-ec2-admin-name.png) | Creating user group — entering group name |
| 04 | [04-create-group-ec2-admin-policy.png](./screenshots/04-create-group-ec2-admin-policy.png) | Creating `EC2-Admin` group with `EC2-Admin-Policy` |
| 05 | [05-create-group-ec2-support-policy.png](./screenshots/05-create-group-ec2-support-policy.png) | Creating `EC2-Support` group and searching `AmazonEC2ReadOnlyAccess` |
| 06 | [06-ec2-support-group-created.png](./screenshots/06-ec2-support-group-created.png) | `EC2-Support` group created confirmation |
| 07 | [07-create-group-s3-support-policy.png](./screenshots/07-create-group-s3-support-policy.png) | Creating `S3-Support` group with `AmazonS3ReadOnlyAccess` |
| 08 | [08-task2-complete-user-groups-created.png](./screenshots/08-task2-complete-user-groups-created.png) | All 3 user groups created — Task 2 complete |
| 09 | [09-iam-users-empty.png](./screenshots/09-iam-users-empty.png) | IAM Users page before creating users |
| 10 | [10-create-user1-details.png](./screenshots/10-create-user1-details.png) | Creating `user-1` with console access as an IAM user |
| 11 | [11-create-user1-custom-password.png](./screenshots/11-create-user1-custom-password.png) | Setting custom password `Sim-Password1` for `user-1` |
| 12 | [12-create-user1-password-reset-unchecked.png](./screenshots/12-create-user1-password-reset-unchecked.png) | Unchecking password reset at next sign-in for `user-1` |
| 13 | [13-create-user1-assign-s3-support.png](./screenshots/13-create-user1-assign-s3-support.png) | Adding `user-1` to `S3-Support` group |
| 14 | [14-user1-created-successfully.png](./screenshots/14-user1-created-successfully.png) | `user-1` created successfully screen |
| 15 | [15-users-list-user1.png](./screenshots/15-users-list-user1.png) | Users list showing `user-1` |
| 16 | [16-users-list-before-user2.png](./screenshots/16-users-list-before-user2.png) | Users list before creating `user-2` |
| 17 | [17-create-user2-custom-password.png](./screenshots/17-create-user2-custom-password.png) | Setting custom password `Sim-Password2` for `user-2` |
| 18 | [18-create-user2-set-permissions.png](./screenshots/18-create-user2-set-permissions.png) | Set permissions screen for `user-2` |
| 19 | [19-create-user2-review-ec2-support.png](./screenshots/19-create-user2-review-ec2-support.png) | Review and create `user-2` in `EC2-Support` |
| 20 | [20-user2-retrieve-password-prompt.png](./screenshots/20-user2-retrieve-password-prompt.png) | Retrieve password confirmation modal for `user-2` |
| 21 | [21-users-list-user1-user2.png](./screenshots/21-users-list-user1-user2.png) | Users list showing `user-1` and `user-2` |
| 22 | [22-create-user3-custom-password.png](./screenshots/22-create-user3-custom-password.png) | Setting custom password `Sim-Password3` for `user-3` |
| 23 | [23-create-user3-set-permissions.png](./screenshots/23-create-user3-set-permissions.png) | Set permissions screen for `user-3` |
| 24 | [24-user3-created-successfully.png](./screenshots/24-user3-created-successfully.png) | `user-3` created successfully screen |
| 25 | [25-user2-permissions-ec2-support.png](./screenshots/25-user2-permissions-ec2-support.png) | `user-2` permissions showing `AmazonEC2ReadOnlyAccess` via `EC2-Support` |
| 26 | [26-policy-ec2-readonly-summary.png](./screenshots/26-policy-ec2-readonly-summary.png) | `AmazonEC2ReadOnlyAccess` policy summary |
| 27 | [27-policy-ec2-readonly-json.png](./screenshots/27-policy-ec2-readonly-json.png) | `AmazonEC2ReadOnlyAccess` policy JSON |
| 28 | [28-ec2-admin-group-details.png](./screenshots/28-ec2-admin-group-details.png) | `EC2-Admin` group details before adding `user-3` |
| 29 | [29-ec2-admin-add-users-list.png](./screenshots/29-ec2-admin-add-users-list.png) | Add users to `EC2-Admin` list |
| 30 | [30-ec2-admin-select-user3.png](./screenshots/30-ec2-admin-select-user3.png) | Selecting `user-3` to add to `EC2-Admin` |
| 31 | [31-ec2-admin-user3-added.png](./screenshots/31-ec2-admin-user3-added.png) | `user-3` added to `EC2-Admin` |
| 32 | [32-task4-complete-all-users-assigned.png](./screenshots/32-task4-complete-all-users-assigned.png) | All 3 users assigned to groups — Task 4 complete |
| 33 | [33-iam-dashboard-summary.png](./screenshots/33-iam-dashboard-summary.png) | IAM Dashboard showing `3` groups and `3` users |
| 34 | [34-iam-signin-url-copied.png](./screenshots/34-iam-signin-url-copied.png) | Copying the IAM user sign-in URL |
| 35 | [35-open-incognito-window.png](./screenshots/35-open-incognito-window.png) | Opening a Chrome Incognito window |
| 36 | [36-incognito-paste-signin-url.png](./screenshots/36-incognito-paste-signin-url.png) | Pasting the IAM console sign-in URL in Incognito |
| 37 | [37-iam-signin-page.png](./screenshots/37-iam-signin-page.png) | Sign in as IAM user page |
| 38 | [38-iam-signin-form.png](./screenshots/38-iam-signin-form.png) | Ready to enter IAM credentials |
| 39 | [39-signin-as-user1.png](./screenshots/39-signin-as-user1.png) | Signing in as `user-1` |
| 40 | [40-user1-console-home.png](./screenshots/40-user1-console-home.png) | `user-1` AWS Console Home |
| 41 | [41-user1-s3-buckets-list.png](./screenshots/41-user1-s3-buckets-list.png) | `user-1` viewing Amazon S3 buckets |
| 42 | [42-user1-s3-sim-website-objects.png](./screenshots/42-user1-s3-sim-website-objects.png) | `user-1` viewing objects in `sim-website` bucket |
| 43 | [43-user1-s3-select-file-upload.png](./screenshots/43-user1-s3-select-file-upload.png) | `user-1` selecting `index.html` to test upload permissions |
| 44 | [44-user1-s3-attempt-upload.png](./screenshots/44-user1-s3-attempt-upload.png) | `user-1` attempting S3 upload |
| 45 | [45-user1-s3-upload-access-denied.png](./screenshots/45-user1-s3-upload-access-denied.png) | `user-1` upload failed with `Access Denied` |
| 46 | [46-signin-as-user2.png](./screenshots/46-signin-as-user2.png) | Signing in as `user-2` |
| 47 | [47-user2-console-home.png](./screenshots/47-user2-console-home.png) | `user-2` AWS Console Home |
| 48 | [48-user2-ec2-instances-view.png](./screenshots/48-user2-ec2-instances-view.png) | `user-2` viewing EC2 instances |
| 49 | [49-user2-ec2-attempt-stop-instance.png](./screenshots/49-user2-ec2-attempt-stop-instance.png) | `user-2` attempting to stop `Application server` |
| 50 | [50-user2-ec2-stop-unauthorized.png](./screenshots/50-user2-ec2-stop-unauthorized.png) | `user-2` denied (`ec2:StopInstances` unauthorized) |
| 51 | [51-signin-as-user3.png](./screenshots/51-signin-as-user3.png) | Signing in as `user-3` |
| 52 | [52-user3-ec2-dashboard.png](./screenshots/52-user3-ec2-dashboard.png) | `user-3` EC2 Dashboard |
| 53 | [53-user3-ec2-stop-instance.png](./screenshots/53-user3-ec2-stop-instance.png) | `user-3` stopping `Application server` |
| 54 | [54-user3-ec2-instance-stopped-success.png](./screenshots/54-user3-ec2-instance-stopped-success.png) | `user-3` successfully stopped EC2 instance `i-0aeba3d612be4e6ec` |
| 55 | [55-user3-s3-no-access-initially.png](./screenshots/55-user3-s3-no-access-initially.png) | `user-3` initially without S3 bucket access |
| 56 | [56-iam-dashboard-admin-return.png](./screenshots/56-iam-dashboard-admin-return.png) | Returning to admin IAM Dashboard |
| 57 | [57-s3-support-group-details.png](./screenshots/57-s3-support-group-details.png) | `S3-Support` group details before adding `user-3` |
| 58 | [58-s3-support-add-users.png](./screenshots/58-s3-support-add-users.png) | Add users to `S3-Support` screen |
| 59 | [59-s3-support-user3-added.png](./screenshots/59-s3-support-user3-added.png) | `user-3` added to `S3-Support` (`2` groups) |
| 60 | [60-user3-s3-buckets-now-accessible.png](./screenshots/60-user3-s3-buckets-now-accessible.png) | `user-3` now able to view S3 buckets |
| 61 | [61-user3-console-signout.png](./screenshots/61-user3-console-signout.png) | Signing out of `user-3` session |

</details>
