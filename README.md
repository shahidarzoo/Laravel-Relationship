# Laravel-Relationship

# Controller 
```php

public function index()
    {
        $profiles = Profile::with(['user', 'level'])->where('is_bd_partner', 'Yes')->get();
    }

```
# Profile Model
```php
class Profile extends Model
{
    public function user()
    {
    	return $this->belongsTo(User::class, 'user_id');
    }
    public function level()
    {
    	return $this->belongsTo(Level::class, 'level_id');
    }
}

```
# User Model
```php
class User extends Authenticatable
{
    use Notifiable;
    use HasRoles;

    public function setPasswordAttribute($password)
    {   
        $this->attributes['password'] = bcrypt($password);
    }
    public function userName()
    {   
        return $this->profile->first_name.' '.$this->profile->last_name;
    }

    public function profile()
    {
        return $this->hasOne(Profile::class, 'id');
    }
}
```
# Level Model
```php
class Level extends Model
{
    protected $guarded = [];

    public function profile()
    {
    	return $this->hasOne(Profile::class, 'id');
    }
}
```
# Render data to view
```php
@php $sn = 1; @endphp
@foreach ($profiles as $profile)
<tr>
	<td>{{$sn++}}</td>
	<td>{{ ucfirst($profile->first_name)}}</td>
	<td>{{ucfirst($profile->last_name)}}</td>
	<td>{{$profile->user->email}}</td>
	<td>{{$profile->cnic}}</td>
	<td>{{$profile->phone_no}}</td>
	<td>{{$profile->level->level}}</td>
	<td>{{$profile->limit}}</td>
	<td>{{date('l jS F Y ', strtotime($profile->created_at))}}</td>
	<td>
	<a href="{{ URL::to('dbd-partner/edit/'.$profile->id) }}" class="btn btn-info btn-xs pull-left" style="margin-right: 3px;">Edit</a>
	<a href="{{ URL::to('dbd-partner/destroy/'.$profile->id) }}" class="btn btn-danger btn-xs">Delete</a>

	</td>
</tr>
@endforeach
@else
<td>{{date(' F jS, Y ', strtotime($timetable->date))}}</td>
@endif
<td>{{$timetable->day}}</td>
<td>{{date('h:i:s a', strtotime($timetable->start_time))}}</td>
<td>{{date('h:i:s a', strtotime($timetable->end_time))}}</td>

</tr>
@endforeach

```
