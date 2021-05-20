# OrderingFilterExtened
Extend OrderingFilter from django_filters.\
Allows to use multiple model fields per parameter.


Examples of usage with full control of ordering:

```
from django_filters import FilterSet, CharFilter, DateFromToRangeFilter
from .filter_ex import OrderingFilterExtened

class UserFilter(FilterSet):
    search = CharFilter(method='filter_by_all_name_fields')
    full_name = CharFilter(method='filter_by_full_name')
    email = CharFilter(method='filter_by_email')
    date_joined = DateFromToRangeFilter()
    last_login = DateFromToRangeFilter()

    order_by = OrderingFilterExtened(
        fields_many=(
            ('full_name', ('first_name', 'last_name'), ('-first_name', 'last_name')),
        ),
        fields=(
            ('email', 'email'),  # model field name, parameter name
            ('last_login', 'last_login'),
            ('date_joined', 'date_joined'),
        ),
    )
```

Examples of usage with auto descening ordering:

```
from django_filters import FilterSet, CharFilter, DateFromToRangeFilter
from .filter_ex import OrderingFilterExtened

class UserFilter(FilterSet):
    search = CharFilter(method='filter_by_all_name_fields')
    full_name = CharFilter(method='filter_by_full_name')
    email = CharFilter(method='filter_by_email')
    date_joined = DateFromToRangeFilter()
    last_login = DateFromToRangeFilter()

    order_by = OrderingFilterExtened(
        fields_many=(
            # same as ('full_name', ('first_name', 'last_name'), ('-first_name', '-last_name')),
            ('full_name', ('first_name', 'last_name')), 
        ),
        fields=(
            ('email', 'email'),  # model field name, parameter name
            ('last_login', 'last_login'),
            ('date_joined', 'date_joined'),
        ),
    )
```

Request example:
```
api/v2/users/order_by=-full_name
```
