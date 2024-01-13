# Django Q 

***
We gunna use Q to solve the problem of multi search parameter in django.
***
I had a problem in one of my projects. my app had advance search 
and i had no idea how to handle it because it was just possible with 
many if conditions . this means a lot of code and possible bug 
so after a lot of search i find below solution .

I hope this help someone



```python
    from django.db.models import Q

                # inside  my views
                
                # these are my parameters 
                first = self.request.GET.get('first')
                second = self.request.GET.get('second')
                third = self.request.GET.get('third')
                fourth = self.request.GET.get('fourth')
                
                # create an instance 
                query = Q()
                
                # we put the Qs to the instance
                if first:
                    query &= Q(role__icontains=first)
                if second:
                    query &= Q(service__code=second)
                if third:
                    query &= Q(updated_at__lte=third)
                if fourth:
                    query &= Q(updated_at__gte=fourth)
                    
                # and in last part we put it in search Query
                queryset = Model.objects.filter(query)

```
