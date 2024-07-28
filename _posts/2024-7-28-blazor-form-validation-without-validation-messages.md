---
layout: post
title: Blazor form validation without the validation messages
image: /images/no-message-validation.png
--- 

For the "churchee" Blazor application I'm working on, I wanted to have it so that the submit button only becomes active when the form is valid.

### What I started with 

On a field change the UpdateFormState method was being called, which in turn updated the state of _formInvalid, which set the disabled state on the submit button (I'm using a Radzen submit button).

```

<RadzenButton Disabled="_formInvalid" Text="Submit" ButtonStyle="ButtonStyle.Success" ButtonType="ButtonType.Submit" />

@code {

    private bool _formInvalid;

    private EditContext _editContext { get; set; }

    protected override void OnParametersSet()
    {
        _formInvalid = false;
        _editContext = new EditContext(InputModel);
    }

    //Triggered when a form field is changed
    private void UpdateFormState()
    {
        _formInvalid = !_editContext.Validate();

        StateHasChanged();
    }
 }

```

### The Problem: Validate triggers validation messages as well as checking state

The Validate method on the EditContext not only checks that every property on the object is in a valid state, it also causes the validation error messages to show if anything is wrong. So after filling out a single field the user is informed that every field is in an invalid state. To which the user could rightly complain... "I haven't got there yet, chill out!".


### Investigation

I figured the best thing to do would be to investigate the source code and see what's happening. I found that the Validate method ends up calling OnValidationRequested which is part of [EditContextDataAnnotationsExtensions](https://github.com/dotnet/aspnetcore/blob/main/src/Components/Forms/src/EditContextDataAnnotationsExtensions.cs)

I discovered the first three lines validate the object, it then moves on to adding the messages, but thoose first three lines are just what I was looking for.

### Solution

I injected the required IServiceProvider and used the three lines found above. Happy days! The submit button becomes active only when the form state is valid but the error messages only show for the fields that have been changed as oposed to the whole form on any change. Much better!

```

<RadzenButton Disabled="_formInvalid" Text="Submit" ButtonStyle="ButtonStyle.Success" ButtonType="ButtonType.Submit" />

@code {

    [Inject]
    public IServiceProvider _serviceProvider { get; set; }

    private bool _formInvalid;

    private EditContext _editContext { get; set; }

    protected override void OnParametersSet()
    {
        _editContext = new EditContext(InputModel);
        SetFormInvalidState();
    }

    //Triggered when a form field is changed
    private void SetFormInvalidState()
    {
        var validationResults = new List<ValidationResult>();
        var validationContext = new ValidationContext(_editContext.Model, _serviceProvider, items: null);
        Validator.TryValidateObject(_editContext.Model, validationContext, validationResults, true);

        _formInvalid = validationResults.Any();

        StateHasChanged();
    }
 }

```

