views.py
def delete_course(request):
    courses = Course.objects.all()
    if request.POST:
    
        course = Course.objects.filter(id__in = request.POST.getlist('items'))
        for cid in course:
            cid.delete()
        return render(request, 'manager/courses.html',{'courses':courses})
    
    else:
        return render(request,'manager/courses.html',{'courses':courses})

courses.html
<div>
        {% if form %}
            <form action="" method="POST">{% csrf_token %}
                {{form.as_p}}
                <input type="submit" value="save course" />
            </form>
        {% endif %}

        {%if success %}
            <h3>The course was successfully saved </h3>
        {% endif %}

        <form action="" method="POST">{% csrf_token %}
        {% if courses %}
            <table id="tb_course" style="border:1px">
                <tr>
                    <th>select</th>
                    <th>#</th>
                    <th>Name</th>
                    <th>Code</th>
                </tr>
            {% for course in courses %}
                <tr>
                    <td><input type="checkbox"  value="{{ course.id }}"   name="items" id="item"></td>
                    <td>{{forloop.counter}}-</td>
                    <td>{{course.name}}</td>
                    <td>{{course.code}}</td>
                    <td><a href="{% url 'manager:edit_course' course.id %}">Edit </a></td>
                </tr>
            {% endfor %}
            </table>

                <a  href="{% url 'manager:delete_course'   %}">
                <input type="button" value="Delete" name="'delete_course" /></a>
            {% endif %}
        </form>

        </div>
