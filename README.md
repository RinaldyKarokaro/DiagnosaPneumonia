
### Login
If you are not logged in you can only access this page or the Sign Up page. The default url takes you to the login page where you use the default credentials **admin@softui.com** with the password **secret**. Logging in is possible only with already existing credentials. For this to work you should have run the migrations. 

The `App\Http\Livewire\Auth\Login` handles the logging in of an existing user.

```
    public function login() {
        $credentials = $this->validate();
        if(auth()->attempt(['email' => $this->email, 'password' => $this->password], $this->remember_me)) {
            $user = User::where(["email" => $this->email])->first();
            auth()->login($user, $this->remember_me);
            return redirect()->intended('/dashboard-default');        
        }
        else{
            return $this->addError('email', trans('auth.failed')); 
        }
    }
```

### Register
You can register as a user by filling in the name, email and password for your account. You can do this by accessing the sign up page from the "**Sign Up**" button in the top navbar or by clicking the "**Sign Up**" button from the bottom of the log in form.. Another simple way is adding **/sign-up** in the url.

The `App\Http\Livewire\Auth\SignUp` handles the registration of a new user.

```
    public function register() {
        $this->validate();
        $user = User::create([
            'name' => $this->name,
            'email' => $this->email,
            'password' => Hash::make($this->password)
        ]);
        auth()->login($user);
        return redirect('/dashboard');
    }
```

### Forgot Password
If a user forgets the account's password it is possible to reset the password. For this the user should click on the "**here**" under the login form or add **/forgot-password** in the url.

The `App\Http\Livewire\Auth\ForgotPassword` takes care of sending an email to the user where he can reset the password afterwards.

```
    public function recoverPassword() { 
        $this->validate();
        $user = User::where('email', $this->email)->first();
        if($user){
            $this->notify(new ResetPassword($user->id));
            $this->showSuccesNotification = true;
            $this->showFailureNotification = false;
        } else {
            $this->showFailureNotification = true;
        }
    }
```

### Reset Password
The user who forgot the password gets an email on the account's email address. The user can access the reset password page by clicking the button found in the email. The link for resetting the password is available for 12 hours. The user must add the email, the password and confirm the password for his password to be updated.

The `App\Http\Livewire\Auth\ResetPassword` helps the user reset the password.

```
    public function resetPassword() {
        $this->validate();
        $existingUser = User::where('email', $this->email)->first();
        if($existingUser && $existingUser->id == $this->urlID) { 
            $existingUser->update([
                'password' => Hash::make($this->password) 
            ]);
            $this->showSuccesNotification = true;
            $this->showFailureNotification = false;
        } else {
            $this->showFailureNotification = true;
        }
    }
```

### User Profile
The profile can be accessed by a logged in user by clicking "**User Profile**" from the sidebar or adding **/user-profile** in the url. The user can add information like phone number, location, description or change the name and email.

The `App\Http\Livewire\UserProfile` handles the user's profile information.

```
    public function save() {
        $this->validate();
        $this->user->save();
        $this->showSuccesNotification = true;
    }
```

### Dashboard
You can access the dashboard either by using the "**Dashboard**" link in the left sidebar or by adding **/dashboard** in the url after logging in. 

## File Structure
```
app
├── Console
│   └── Kernel.php
├── Exceptions
│   └── Handler.php
├── Http
│   ├── Controllers
│   │   └── Controller.php
│   ├── Kernel.php
│   ├── Livewire
│   │   ├── Auth
│   │   │   ├── ForgotPassword.php
│   │   │   ├── Login.php
│   │   │   ├── Logout.php
│   │   │   ├── ResetPassword.php
│   │   │   └── SignUp.php
│   │   ├── Billing.php
│   │   ├── Dashboard.php
│   │   ├── LaravelExamples
│   │   │   ├── UserManagement.php
│   │   │   └── UserProfile.php
│   │   ├── Profile.php
│   │   ├── Rtl.php
│   │   ├── StaticSignIn.php
│   │   ├── StaticSignUp.php
│   │   └── Tables.php
│   └── Middleware
│       ├── Authenticate.php
│       ├── EncryptCookies.php
│       ├── PreventRequestsDuringMaintenance.php
│       ├── RedirectIfAuthenticated.php
│       ├── TrimStrings.php
│       ├── TrustHosts.php
│       ├── TrustProxies.php
│       └── VerifyCsrfToken.php
├── Models
│   └── User.php
├── Notifications
│   └── ResetPassword.php
├── Providers
│   ├── AppServiceProvider.php
│   ├── AuthServiceProvider.php
│   ├── BroadcastServiceProvider.php
│   ├── EventServiceProvider.php
│   └── RouteServiceProvider.php
└── View
    └── Components
        └── Layouts
            ├── App.php
            └── ...
```
## Browser Support
At present, we officially aim to support the last two versions of the following browsers:

<img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/chrome.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/firefox.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/edge.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/safari.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/opera.png" width="64" height="64">

## Reporting Issues
We use GitHub Issues as the official bug tracker for the Soft UI Dashboard. Here are some advices for our users that want to report an issue:

1. Make sure that you are using the latest version of the Soft UI Dashboard. Check the CHANGELOG from your dashboard on our [website](https://www.creative-tim.com/product/soft-ui-dashboard-laravel?ref=readme-sudl).
2. Providing us reproducible steps for the issue will shorten the time it takes for it to be fixed.
3. Some issues may be browser specific, so specifying in what browser you encountered the issue might help.

## Licensing
- Copyright 2021 [Creative Tim](https://www.creative-tim.com?ref=readme-sudl)
- Creative Tim [license](https://www.creative-tim.com/license?ref=readme-sudl)

## Useful Links
- [Tutorials](https://www.youtube.com/channel/UCVyTG4sCw-rOvB9oHkzZD1w)
- [Affiliate Program](https://www.creative-tim.com/affiliates/new) (earn money)
- [Blog Creative Tim](http://blog.creative-tim.com/)
- [Free Products](https://www.creative-tim.com/bootstrap-themes/free) from Creative Tim
- [Premium Products](https://www.creative-tim.com/bootstrap-themes/premium?ref=sudl-readme) from Creative Tim
- [React Products](https://www.creative-tim.com/bootstrap-themes/react-themes?ref=sudl-readme) from Creative Tim
- [VueJS Products](https://www.creative-tim.com/bootstrap-themes/vuejs-themes?ref=sudl-readme) from Creative Tim
- [More products](https://www.creative-tim.com/bootstrap-themes?ref=sudl-readme) from Creative Tim
- Check our Bundles [here](https://www.creative-tim.com/bundles??ref=sudl-readme)

