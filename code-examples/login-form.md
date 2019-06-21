# Login Form

## Controllers

```csharp
public class MembershipController : Umbraco.Web.Mvc.SurfaceController
    {
        [HttpGet]
        [ActionName("MemberLogin")]
        public ActionResult Index()
        {
            return PartialView("LoginForm", new LoginViewModel());
        }

        [HttpGet]
        public ActionResult Logout()
        {
            Session.Clear();
            FormsAuthentication.SignOut();
            return Redirect("/");
        }

        [HttpPost]
        [ActionName("MemberLogin")]
        public ActionResult Validate(LoginViewModel model)
        {
            if (Membership.ValidateUser(model.Login, model.Password))
            {
                FormsAuthentication.SetAuthCookie(model.Login, model.RememberMe);
                return RedirectToCurrentUmbracoPage();
            }
            
            TempData["Status"] = "Invalid Log-in Credentials";
            return RedirectToCurrentUmbracoPage();
        }
    }
```

## View

```html
@using JondJonesSampleSite.Controllers;
@model JonDJones.BusinessLayer.ViewModel.LoginViewModel

@if (User.Identity.IsAuthenticated)
{
    <p>
        Welcome, @User.Identity.Name
    </p>
    <p>
        @Html.ActionLink("Log out", "Logout", "Membership")
    </p>
}
else
{
    using (Html.BeginUmbracoForm<MembershipController>("MemberLogin"))
    {
        @Html.EditorFor(x => Model)
        <input type="submit" />
    }

    <p>@TempData["Status"]</p>
}   
```

## View Model

```csharp
public class LoginViewModel
{
    public string Login { get; set; }

    public string Password { get; set; }

    public bool RememberMe { get; set; }
}
```

---
[:arrow_left: BACK](../README.md)
```