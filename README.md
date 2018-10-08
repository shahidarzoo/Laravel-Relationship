# Laravel-Relationship

# Controller 
```php

public function index()
    {
        $school = auth()->user()->school()->first();
        $subjects = Subject::select('id','subject_name')->where('school_id',$school->id)->get();
        $teachers = $school->teachers()->with('user')->get();
        $timetables = ClassTimeTable::with(['subject','teacher','teacher.user'])->where('school_id',$school->id)->get();
        return view('schedules.timetables', compact('subjects','timetables','teachers'));
    }

```
# Subject Model
```php
public function timeTables() {
        return $this->hasMany(ClassTimeTable::class, 'subject_id');
    }
```
# Teacher Model
```php
public function timeTables() {
        return $this->hasMany(ClassTimeTable::class,'teacher_id');
    }
```
# User Model
```php
 public function teacher() {
        return $this->hasOne('App\Teacher');
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
