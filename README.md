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
@foreach($timetables as $timetable)
<tr>
<td>{{$sn++}}</td>
<td>{{$timetable->description}}</td>
<td>
{{$timetable->type}}
</td>
<td>{{$timetable->subject->subject_name}}</td>
@if($timetable->teacher->user->avatar == '')
<td><img class="img-circle img-xs" src="img/profile-photos/1.png"></td>
@else
<td><img class="img-circle img-xs" src="{{$timetable->teacher->user->avatar}}"></td>
@endif
<td>{{$timetable->teacher->user->first_name}} {{$timetable->teacher->user->last_name}}</td>
@if($timetable->date == '0000-00-00')
<td></td>
@else
<td>{{date(' F jS, Y ', strtotime($timetable->date))}}</td>
@endif
<td>{{$timetable->day}}</td>
<td>{{date('h:i:s a', strtotime($timetable->start_time))}}</td>
<td>{{date('h:i:s a', strtotime($timetable->end_time))}}</td>

</tr>
@endforeach

```
