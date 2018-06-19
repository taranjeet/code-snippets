## Third Party Package

### Django Allauth

#### Use pre_social_login signal to allow emails of only specific domain to sign in

```
from django.contrib.auth.models import User
from django.http import HttpResponseRedirect
from django.core.urlresolvers import reverse
from django.contrib import messages

from allauth.socialaccount.adapter import DefaultSocialAccountAdapter
from allauth.exceptions import ImmediateHttpResponse
from allauth.account.utils import perform_login
from allauth.account import app_settings


class LoginAdapter(DefaultSocialAccountAdapter):

    def pre_social_login(self, request, sociallogin):
        email = sociallogin.account.extra_data['email']
        if not email.split('@')[-1] == 'allowdomain.com':
            messages.add_message(request, messages.ERROR, 'Sorry! Only user with @allowdomain.com email can login')
            raise ImmediateHttpResponse(HttpResponseRedirect(reverse('admin:login')))
        try:
            user = sociallogin.account.user
        except Exception as e:
            user = None
        if user and user.id:
            return
        try:
            existing_user = User.objects.get(email=email)
            sociallogin.connect(request, existing_user)
        except User.DoesNotExist:
            pass
        else:
            perform_login(request, existing_user, app_settings.EmailVerificationMethod.NONE)

# in project settings
SOCIALACCOUNT_ADAPTER = 'path.to.LoginAdapter'
```
