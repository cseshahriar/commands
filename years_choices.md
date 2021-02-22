# models 

  import datetime

  year_dropdown = []
  for y in range(2011, (datetime.datetime.now().year + 5)):
      year_dropdown.append((y, y))

  year = models.IntegerField(_('year'), max_length=4, choices=year_dropdown, default=datetime.datetime.now().year)


# forms 

  <select id="year">
  {% for y in range(2011, (datetime.datetime.now().year + 5)) %}
      <option value="{{ y }}">{{ y }}</option>
  {% endfor %}
  </select>
