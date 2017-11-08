# !/usr/bin/env python
# -*- coding:utf8 -*-
#  Created: 2017-11-08
#  Modified: 2017-11-09
#  Author: sherwinwangs
#  Version 1.1

import sys
import math


# change addr to dec
def addr2dec(addr):
    items = [int(str(x)) for x in addr.split('.')]
    return sum([items[i] << [24, 16, 8, 0][i] for i in range(4)])


# dec to addr
def dec2addr(dec):
    return '.'.join([str(dec >> x & 0xff) for x in [24, 16, 8, 0]])


def ipstring_to_cidr(start, end):
    '''
    :desc:split ip to mode like 127.0.0.0/24
    :param start: start ip address
    :param end:  end ip address
    :return:cidrlist
    '''
    cidr2mask = [0x00000000, 0x80000000, 0xC0000000,
                 0xE0000000, 0xF0000000, 0xF8000000,
                 0xFC000000, 0xFE000000, 0xFF000000,
                 0xFF800000, 0xFFC00000, 0xFFE00000,
                 0xFFF00000, 0xFFF80000, 0xFFFC0000,
                 0xFFFE0000, 0xFFFF0000, 0xFFFF8000,
                 0xFFFFC000, 0xFFFFE000, 0xFFFFF000,
                 0xFFFFF800, 0xFFFFFC00, 0xFFFFFE00,
                 0xFFFFFF00, 0xFFFFFF80, 0xFFFFFFC0,
                 0xFFFFFFE0, 0xFFFFFFF0, 0xFFFFFFF8,
                 0xFFFFFFFC, 0xFFFFFFFE, 0xFFFFFFFF]
    startaddr = addr2dec(start)
    endaddr = addr2dec(end)
    if startaddr > endaddr:
        print "start is bigger than end!!!"
        sys.exit()
    cidrlist = []
    while endaddr >= startaddr:
        maxsize = 32
        while maxsize > 0:
            mask = cidr2mask[maxsize - 1]
            maskedbase = startaddr & mask
            if maskedbase != startaddr:
                break
            maxsize -= 1
        x = math.log(endaddr - startaddr + 1) / math.log(2)
        maxdiff = 32 - float(x)
        if maxsize < maxdiff:
            maxsize = maxdiff

        cidrlist.append(dec2addr(startaddr) + "/" + str(int(maxsize)))
        startaddr += 2 ** (32 - maxsize)
    return cidrlist


# print ipstring_to_cidr('001.000.008.000', '001.000.015.255')

def write_to_file(str):
    with open('/Users/sherwin/Downloads/home/codebase/17mon/mydata4vipday2-cidr3.txtx', 'a') as f1:
        f1.write(str)


def main():
    with open('/Users/sherwin/Downloads/home/codebase/17mon/mydata4vipday2.txtx', 'r') as f:
        tmp = f.readlines()
    for i in tmp:
        line = i.split('\t')
        ip_start = line[0]
        ip_end = line[1]
        ip_comment = line[2:]
        result = ipstring_to_cidr(ip_start, ip_end)
        for i in result:
            ip_line = []
            ip_line.append(i)
            ip_line += ip_comment
            # print ip_line
            # print len(ip_line)
            write_to_file('%s %s %s %s %s %s %s %s %s %s %s %s %s %s' % tuple(ip_line))


if __name__ == '__main__':
    main()
