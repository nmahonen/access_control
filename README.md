# access_control
Project for school, learning to use class and objects


DOORCODES = {'TC114': ['TIE'], 'TC203': ['TIE'], 'TC210': ['TIE', 'TST'],
             'TD201': ['TST'], 'TE111': [], 'TE113': [], 'TE115': [],
             'TE117': [], 'TE102': ['TIE'], 'TD203': ['TST'], 'TA666': ['X'],
             'TC103': ['TIE', 'OPET', 'SGN'], 'TC205': ['TIE', 'OPET', 'ELT'],
             'TB109': ['OPET', 'TST'], 'TB111': ['OPET', 'TST'],
             'TB103': ['OPET'], 'TB104': ['OPET'], 'TB205': ['G'],
             'SM111': [], 'SM112': [], 'SM113': [], 'SM114': [],
             'S1': ['OPET'], 'S2': ['OPET'], 'S3': ['OPET'], 'S4': ['OPET'],
             'K1705': ['OPET'], 'SB100': ['G'], 'SB202': ['G'],
             'SM220': ['ELT'], 'SM221': ['ELT'], 'SM222': ['ELT'],
             'secret_corridor_from_building_T_to_building_F': ['X', 'Y', 'Z'],
             'TA': ['G'], 'TB': ['G'], 'SA': ['G'], 'KA': ['G']}

class Accesscard:
    """
    This class models an access card which can be used to check
    whether a card should open a particular door or not.
    """

    def __init__(self, id, name):
        """
        Constructor, creates a new object that has no access rights.

        :param id: str, card holders personal id
        :param name: str, card holders name

        THIS METHOD IS AUTOMATICALLY TESTED, DON'T CHANGE THE NAME OR THE
        PARAMETERS!
        """

        self.__identifier = id
        self.__name = name
        self.__access_codes = []

    def info(self):
        """
        The method has no return value. It prints the information related to
        the access card in the format:
        id, name, access: a1,a2,...,aN
        for example:
        777, Thelma Teacher, access: OPET, TE113, TIE
        Note that the space characters after the commas and semicolon need to
        be as specified in the task description or the test fails.

        THIS METHOD IS AUTOMATICALLY TESTED, DON'T CHANGE THE NAME, THE
        PARAMETERS, OR THE PRINTOUT FORMAT!
        """

        print(self.__identifier, ", ", self.__name, ", access: ",
              ", ".join([str(x) for x in sorted(self.__access_codes)]), sep="")

        #tässä on tulostus kai nyt ok muodossa

    def get_name(self):
        """
        :return: Returns the name of the accesscard holder.
        """

        return self.__name

        # tämä vissiin ok, mutta tarviiko mihinkään?
        # jos ei, niin voiko poistaa vaikka tämä oli alkuperäisessä koodipohjassa?

    def add_access(self, new_access_code):
        """
        The method adds a new accesscode into the accesscard according to the
        rules defined in the task description.

        :param new_access_code: str, the accesscode to be added in the card.

        THIS METHOD IS AUTOMATICALLY TESTED, DON'T CHANGE THE NAME, THE
        PARAMETERS, OR THE RETURN VALUE! DON'T PRINT ANYTHING IN THE METHOD!
        """
        # jos koodi löytyy jo oliolta, niin palautetaan vaan entinen lista
        if new_access_code in self.__access_codes:
            return self.__access_codes

        # tarkistetaan aluekoodien ja yksittäisten koodien päällekkäisyydet
        # ja poistetaan yksittäiset ovikoodit jos päällekkäisyyttä on
        elif new_access_code in DOORCODES:
            for code in DOORCODES[new_access_code]:
                if code in self.__access_codes:
                    self.__access_codes.remove(code)

        # lisätään koodi oliolle
        self.__access_codes.append(new_access_code)

        # liekö tämä add_access nyt oikeasti ok, vai pitääkö tätä säätää
        # että saa sen command == add -kohdan oikein?
        # ainakin joku on vikana tuossa for-loopissa, koska se ottaa pois
        # aluekoodin jos yrittää lisätä ovea, joka aluekoodilla avautuu

    def check_access(self, door):
        """
        Checks if the accesscard allows access to a certain door.

        :param door: str, the doorcode of the door that is being accessed.
        :return: True: The door opens for this accesscard.
                 False: The door does not open for this accesscard.

        THIS METHOD IS AUTOMATICALLY TESTED, DON'T CHANGE THE NAME, THE
        PARAMETERS, OR THE RETURN VALUE! DON'T PRINT ANYTHING IN THE METHOD!
        """

        for key in DOORCODES:
            if door in DOORCODES[key]:
                if door in self.__access_codes:
                    return True

        if door not in self.__access_codes:
            return False
        else:
            return True

        # joku on vielä pielessä, kun ei ymmärrä aluekoodin antamia
        # pääsyjä yksittäisiin ovikoodeihin?

    def merge(self, card):
        """
        Merges the accesscodes from another accesscard to this accesscard.

        :param card: Accesscard, the accesscard whose access rights are added to this card.

        THIS METHOD IS AUTOMATICALLY TESTED, DON'T CHANGE THE NAME, THE
        PARAMETERS, OR THE RETURN VALUE! DON'T PRINT ANYTHING IN THE METHOD!
        """
        # luodaan kaksi joukkoa: ensimmäinen sille kortille, johon lisätään ja
        # toinen sille kortille, josta koodeja lisätään
        self_set = set()
        other_set = set()

        # käydään viemässä kaikki ensimmäisen kortin koodit ensimmäiseen
        # joukkoon
        for code in self.__access_codes:
            self_set.add(code)
        # käydään viemässä kaikki toisen kortin koodit toiseen joukkoon
        for code in card.__access_codes:
            other_set.add(code)

        # käydään läpi listojen eroavat koodit ja lisätään ne eroavat koodit
        # self.acces_codes-listaan
        for code in (other_set - self_set):
            self.__access_codes.append(code)

        # onko tää nyt ok näin, vai pitääkö huomioida aluekoodit jotenkin
        # erikseen?


# TODO: Implement helper functions here.
# tarviiko apufunktioita?

def main():

    filename = "accessinfo.txt"

    try:
        file = open(filename, mode="r")
        organization_dict = {}

        # kun tiedosto saadaan avatuksi, se käydään rivi riviltä läpi
        # ja jaetaan osiin, jolloin näistä osista tulee id, nimi ja avainkoodit
        # avainkoodit pitää jakaa vielä omaksi listakseen
        for line in file:
            line = line.rstrip()
            splitted_line = line.split(";")
            id_num = splitted_line[0]
            name = splitted_line[1]
            keys = splitted_line[2]
            key_list = keys.split(",")

            # luodaan kortti-oliot ja lisätään niille avainkoodit
            card = Accesscard(id_num, name)
            for key in key_list:
                card.add_access(key.strip())

            # dictiin id-numero avaimeksi ja oliot arvoksi
            organization_dict[id_num] = card

        while True:
            line = input("command> ")

            if line == "":
                break

            strings = line.split()
            command = strings[0]

            if command == "list" and len(strings) == 1:
                for id_number in sorted(organization_dict):
                    organization_dict[id_number].info()

                # list taitaa olla ok

            elif command == "info" and len(strings) == 2:
                card_id = strings[1]

                if card_id in organization_dict:
                    organization_dict[card_id].info()
                else:
                    print("Error: unknown id.")

                # info taitaa olla ok

            elif command == "access" and len(strings) == 3:
                card_id = strings[1]
                door_id = strings[2]

                if card_id not in organization_dict:
                    print("Error: unknown id.")
                elif door_id in DOORCODES:
                    if organization_dict[card_id].check_access(door_id) is True:
                        print("Card ", card_id, " ( ",
                        organization_dict[card_id].get_name(), " ) "
                        "has access to door ", door_id, sep="")
                    elif organization_dict[card_id].check_access(door_id) is False:
                        print("Card ", card_id, " ( ",
                        organization_dict[card_id].get_name(), " ) "
                        "has no access to door ", door_id, sep="")
                else:
                    print("Error: unknown doorcode.")

                # ymmärtääkseni tämäkin access on tässä kohtaa ok?
                # check_access työn alla...

            elif command == "add" and len(strings) == 3:
                card_id = strings[1]
                access_code = strings[2]

                if card_id not in organization_dict:
                    print("Error: unknown id.")
                elif access_code in DOORCODES:
                    for key in DOORCODES[access_code]:
                        if access_code == key:
                            organization_dict[card_id].add_access(access_code)
                    else:
                        organization_dict[card_id].add_access(access_code)
                else:
                    print("Error: unknown doorcode.")

                # aluekoodin lisäys?

            elif command == "merge" and len(strings) == 3:
                card_id_to = strings[1]
                card_id_from = strings[2]
                if card_id_to not in organization_dict:
                    print("Error: unknown id.")
                elif card_id_from not in organization_dict:
                    print("Error: unknown id.")
                else:
                    organization_dict[card_id_to].merge(organization_dict[card_id_from])

                # merge vissiin ok?

            elif command == "quit":
                print("Bye!")
                return
            else:
                print("Error: unknown command.")

        file.close()

    except OSError:
        print("Error: file cannot be read.")

if __name__ == "__main__":
    main()

