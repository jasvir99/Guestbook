from django.core.cache import cache
#from django.views.generic import templateView
from guestbook.forms import CreateGreetingForm
from guestbook.models import Greeting
from django.http import HttpResponseRedirect
from django.shortcuts import render_to_response
def list_greetings(request):
    greetings = cache.get(MEMCACHE_GREETINGS)
    if greetings is None:
        greetings = Greeting.objects.all().order_by('-date')[:10]
        cache.add(MEMCACHE_GREETINGS, greetings)
    return render_to_response('guestbook/index.html',
        {'greetings': greetings, 'form': CreateGreetingForm()})

def create_greeting(request):
    # bound form (user input in request)
    if request.method == 'POST':
        form = CreateGreetingForm(request.POST)
        if form.is_valid():
            greeting = form.save(commit=False)
            if request.user.is_authenticated():
                greeting.author = request.user
            greeting.save()
            cache.delete('greetings')
    return HttpResponseRedirect('/guestbook/')

