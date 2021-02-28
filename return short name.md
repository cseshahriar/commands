@property
    def short_name(self):
        """ Return short name"""
        full_name = self.name_en.lower()
        t_list = full_name.split()
        name = ""
        for word in t_list:
            name += word[0]
        return name.upper()
